---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - syntax
  - best-practice
aliases:
  - class
  - class type
  - class invariant
  - implicit object
up: "[[Chapter 14 — Introduction to Classes]]"
related:
  - "[[Member Functions]]"
  - "[[Const Member Functions]]"
  - "[[Member Access]]"
  - "[[Data Hiding]]"
  - "[[Constructors]]"
  - "[[Object-oriented Programming]]"
  - "[[Structs]]"
  - "[[Program-defined Types]]"
  - "[[Initialization]]"
  - "[[One Definition Rule]]"
---

# Classes

A **class** is a program-defined type (like a [[Structs|struct]]) that groups data members and member functions together. The key difference from `struct`: **class members are private by default**.

```cpp
class Date
{
public:
    int m_day{};
    int m_month{};
    int m_year{};
};
```

A **class invariant** is a condition that must be true throughout the lifetime of an object for it to remain in a valid state. Classes enforce invariants through [[Constructors|constructors]] and [[Member Access|private data]].

## Class vs struct

The **only** formal difference between `class` and `struct` in C++:

| | `struct` | `class` |
|--|---------|---------|
| Default member access | `public` | `private` |
| Default inheritance | `public` | `private` |

By convention: use `struct` for passive data holders (aggregates); use `class` when you need invariants, encapsulation, or constructors (see [[Data Hiding]]).

If a class type has no data members, prefer using a namespace instead.

## Subnodes

- [[Member Functions]] — how member functions work and the implicit object
- [[Const Member Functions]] — member functions callable on `const` objects
- [[Member Access]] — public/private/protected access specifiers and access functions

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Classes
