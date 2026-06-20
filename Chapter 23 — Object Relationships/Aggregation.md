---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - best-practice
  - subnode
aliases:
  - aggregation
  - has-a relationship
  - non-owning member
up: "[[Object Composition]]"
related:
  - "[[Object Composition]]"
  - "[[Composition]]"
  - "[[Association]]"
  - "[[std-reference-wrapper|std::reference_wrapper]]"
  - "[[Lvalue References]]"
  - "[[Classes]]"
  - "[[Chapter 23 — Object Relationships]]"
---

# Aggregation

**Aggregation** is a subtype of [[Object Composition]] where the containing class holds a reference to a part that it **does not own**. The part has an independent lifetime and may be shared across multiple containing objects.

## Qualifying rules

All four conditions must hold:

1. The part is **part of** the object (class).
2. The part can (if desired) **belong to more than one object** at a time.
3. The part's **existence is not managed** by the object — the part is created and destroyed externally.
4. The part **does not know** about the existence of the containing object.

## Classic example

A car and its engine: the engine is part of the car, but the engine could also belong to other contexts (the owner, a garage), and the car is not responsible for creating or destroying the engine.

## Implementation

Aggregation typically uses **pointer or reference members** pointing to objects that live outside the class scope:

```cpp
class Engine { /* ... */ };

class Car
{
    const Engine* m_engine; // pointer to externally managed engine — Car does not own it
public:
    Car(const Engine* engine) : m_engine{ engine } {}
    // No delete in destructor — Car didn't allocate the engine
};
```

## Storing references in containers: std::reference_wrapper

Raw [[Lvalue References|references]] cannot be stored in standard containers (`std::vector`, etc.) because they are not copyable/assignable. Use [[std-reference-wrapper|std::reference_wrapper]] instead:

```cpp
std::vector<std::reference_wrapper<Engine>> engines { eng1, eng2 };
engines[0].get().start(); // access via .get()
```

`std::reference_wrapper` acts like a reference but satisfies container requirements. Note: the wrapped object must not be a temporary — the reference would dangle.

## Design guidance

Implement the **simplest relationship type** that meets your program's needs. Don't model real-world ownership if your code doesn't need it — aggregation is appropriate whenever the part outlives or exists independently of the containing object.

> Full coverage: [[Chapter 23 — Object Relationships]] → Object Composition → Aggregation
