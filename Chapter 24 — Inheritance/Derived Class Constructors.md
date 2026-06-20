---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - syntax
aliases:
  - derived constructor
  - base constructor call
  - inherited constructor
  - constructor chaining
  - derived class initialization
  - destruction order
up: "[[Chapter 24 — Inheritance]]"
related:
  - "[[Basic Inheritance]]"
  - "[[Inheritance and Access Specifiers]]"
  - "[[Constructors]]"
  - "[[Destructors]]"
  - "[[Member Initializer List]]"
  - "[[Default Constructor]]"
  - "[[Delegating Constructors]]"
  - "[[Chapter 24 — Inheritance]]"
---

# Derived Class Constructors

When a derived class object is instantiated, C++ must construct **both** the base portion and the derived portion. These are constructed in a fixed sequence, and the derived class controls which base constructor runs via the [[Member Initializer List]].

## Base class instantiation sequence

1. Memory for the object is allocated (enough for both the Base and Derived portions).
2. The appropriate **Derived** constructor is called.
3. **The Base portion is constructed first** — using the base constructor specified in the member initializer list, or the [[Default Constructor]] if none is specified.
4. The member initializer list initializes Derived's own variables.
5. The body of the Derived constructor executes.
6. Control returns to the caller.

## Calling a specific base constructor

To initialize base members that have no public setter, forward arguments to the base constructor in the [[Member Initializer List]]:

```cpp
Derived(double cost = 0.0, int id = 0)
    : Base{ id }        // explicitly call Base(int)
    , m_cost{ cost }
{
}
```

Without `: Base{ id }`, C++ would call the [[Default Constructor|default constructor]] of Base, leaving base-only members uninitialized.

## Destruction order

Destructors run in the **reverse order of construction** — most-derived first, working up to the most-base:

```cpp
// If A ← B ← C:
// Construction:  A → B → C
// Destruction:   C → B → A
```

Each class's [[Destructors|destructor]] cleans up only the members belonging to that layer.

## Multi-level chains

In a chain `A ← B ← C`, constructing `C` triggers:
- `C` constructor calls → `B` constructor calls → `A` constructor (runs first)
- Then `B` body runs, then `C` body runs

The same chained-call mechanism applies no matter how deep the hierarchy goes.

> Full coverage: [[Chapter 24 — Inheritance]] → Derived Class Constructors
