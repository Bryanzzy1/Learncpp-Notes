---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - syntax
aliases:
  - inheritance
  - base class
  - derived class
  - parent class
  - child class
  - superclass
  - subclass
  - is-a relationship
up: "[[Chapter 24 — Inheritance]]"
related:
  - "[[Derived Class Constructors]]"
  - "[[Inheritance and Access Specifiers]]"
  - "[[Object-oriented Programming]]"
  - "[[Classes]]"
  - "[[Composition]]"
  - "[[Chapter 24 — Inheritance]]"
---

# Basic Inheritance

**Inheritance** is a C++ mechanism that allows a new class (the **derived class**) to acquire the members — functions and variables — of an existing class (the **base class**) and extend or specialize them.

## Terminology

| Term | Synonyms |
|---|---|
| Class being inherited from | base class, parent class, superclass |
| Class doing the inheriting | derived class, child class, subclass |

## Syntax

```cpp
class BaseballPlayer : public Person
{
    // BaseballPlayer automatically has all of Person's members,
    // plus any new members defined here
};
```

The `: public Person` establishes a **public inheritance** relationship — the most common kind. See [[Inheritance and Access Specifiers]] for alternatives.

## What is inherited

A derived class automatically receives:
- All member **variables** of the base class.
- All member **functions** of the base class.

The derived class can then add new members or override existing ones on top of this foundation.

## Relationship to object composition

Inheritance models an **is-a** relationship (a `BaseballPlayer` *is a* `Person`). This contrasts with [[Composition]], which models a **part-of** relationship. Prefer composition unless a true is-a hierarchy is warranted.

## Order of construction

When a derived object is created, C++ constructs it in phases from the **most-base class downward**:

1. The topmost base class is constructed first.
2. Each derived layer is constructed in order, down to the most-derived class.

```cpp
// If A ← B ← C (C derives from B derives from A):
// Construction order: A, then B, then C
// Destruction order:  C, then B, then A (reverse)
```

Each layer calls its own constructor during this process. See [[Derived Class Constructors]] for how to control which base constructor is called.

> Full coverage: [[Chapter 24 — Inheritance]] → Basic Inheritance
