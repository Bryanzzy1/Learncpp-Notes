---
tags:
  - cpp/types
  - concept
  - syntax
  - best-practice
aliases:
  - program-defined type
  - user-defined type
  - type definition
up: "[[Chapter 13 — Compound Types]]"
related:
  - "[[Enumerations]]"
  - "[[Structs]]"
  - "[[Class Templates]]"
  - "[[One Definition Rule]]"
  - "[[Header Files]]"
  - "[[Translation Unit]]"
---

# Program-defined Types

A **program-defined type** (also called a **user-defined type**) is any type that the programmer defines — as opposed to fundamental types built into the language. In C++, these include:

- `struct`, `class`, and `union` types (collectively **class types**)
- `enum` and `enum class` types ([[Enumerations]])

```cpp
struct Fraction
{
    int numerator {};
    int denominator {};
};
```

## ODR exemption

Type definitions are **partially exempt from the [[One Definition Rule]]**: the same type definition may appear in multiple [[Translation Unit|translation units]] (i.e., `#included` from a [[Header Files|header file]]) as long as every definition is identical. This is what makes it safe to put struct and class definitions in headers.

## Naming conventions

Name program-defined types starting with a capital letter and without a `_t` suffix (which is reserved for standard-library types).

> Full coverage: [[Chapter 13 — Compound Types]] → Program-defined Types
