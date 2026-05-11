---
tags:
  - cpp/types
  - cpp/casting
  - concept
  - subnode
aliases:
  - standard conversion sequences
up: "[[Implicit Type Conversion]]"
related:
  - "[[Implicit Type Conversion]]"
  - "[[Numeric Promotion]]"
  - "[[Numeric Conversions]]"
  - "[[Implicit Conversion]]"
  - "[[Chapter 10 — Type conversion]]"
---

# Standard Conversions

The **standard conversions** define how the compiler implicitly converts fundamental types (and certain compound types — arrays, references, pointers, enumerations) to other types within the same group.

| Category | Meaning |
| --- | --- |
| Numeric promotions | Small integral types → `int`/`unsigned int`; `float` → `double` |
| Numeric conversions | Other integral and floating-point conversions (not promotions) |
| Qualification conversions | Add or remove `const`/`volatile` |
| Value transformations | Change the value category of an expression |
| Pointer conversions | `std::nullptr` → pointer types, pointer types → other pointer types |

Numeric promotions (see [[Numeric Promotion]]) are a subset of the standard conversions and are preferred by the compiler because they are value-preserving. Numeric conversions (see [[Numeric Conversions]]) cover everything else and may be unsafe.

> Full coverage: [[Chapter 10 — Type conversion]] → Standard Conversions
