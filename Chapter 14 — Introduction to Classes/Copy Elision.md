---
tags:
  - cpp/classes
  - cpp/optimization
  - concept
  - subnode
aliases:
  - copy elision
  - elided constructor
  - RVO
  - mandatory copy elision
up: "[[Copy Constructor]]"
related:
  - "[[Copy Constructor]]"
  - "[[Initialization]]"
  - "[[List Initialization]]"
  - "[[Return by Value]]"
  - "[[Pass by Value]]"
---

# Copy Elision

**Copy elision** is a compiler optimization that eliminates unnecessary calls to the [[Copy Constructor|copy constructor]], directly constructing the object in its final location instead.

When the compiler elides a copy constructor call, the constructor is said to have been **elided**.

## Initialization form differences

Three key differences between initialization forms matter in this context:

| Form | Behavior |
|------|----------|
| List initialization | Disallows narrowing conversions; prioritizes list constructors |
| Copy initialization | Only considers non-`explicit` constructors and conversion functions |
| List initialization | Prioritizes matching list constructors over other constructors |

## Where copy elision applies

**Pass by value**: when passing an object by value, the compiler may elide the copy and construct the parameter in-place.

**Return by value**: when a function returns a local object, the compiler may elide the copy and construct the return value directly in the caller's storage (Return Value Optimization, RVO).

## Mandatory copy elision (C++17)

Since C++17, copy elision is **mandatory** in certain contexts — the compiler is **required** to elide the copy, not merely permitted to. This means the copy constructor may not even need to exist for these cases.

The most common mandatory case: when a function returns a **prvalue** (a temporary constructed in the `return` statement), the copy constructor for that temporary is guaranteed to be elided.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Copy Elision
