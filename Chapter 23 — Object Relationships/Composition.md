---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - best-practice
  - subnode
aliases:
  - composition
  - part-of relationship
  - composite object
up: "[[Object Composition]]"
related:
  - "[[Object Composition]]"
  - "[[Aggregation]]"
  - "[[Classes]]"
  - "[[Constructors]]"
  - "[[new and delete]]"
  - "[[Chapter 23 — Object Relationships]]"
---

# Composition

**Composition** is a subtype of [[Object Composition]] where the containing class (the whole) **owns and manages** the lifetime of its parts. The part exists solely as part of the whole.

## Qualifying rules

All four conditions must hold:

1. The part is **part of** the object (class).
2. The part can only belong to **one object at a time**.
3. The part's **existence is managed** by the object — the class creates and destroys the part.
4. The part **does not know** about the existence of the containing object.

## Implementation

Composition typically uses **normal member variables**. Because the part's lifetime is tied to the whole, the class [[Constructors|constructor]] creates the part and the destructor destroys it automatically:

```cpp
class Heart { /* ... */ };

class Person
{
    Heart m_heart; // Person owns its Heart — it's created/destroyed with Person
};
```

Pointer members can also implement composition if the class explicitly handles allocation and deallocation via [[new and delete]].

## Contrast with Aggregation

In [[Aggregation]], the part's lifetime is **independent** of the whole — the class holds a pointer or reference to an externally managed object. In Composition, no such external management occurs.

> Full coverage: [[Chapter 23 — Object Relationships]] → Object Composition → Composition
