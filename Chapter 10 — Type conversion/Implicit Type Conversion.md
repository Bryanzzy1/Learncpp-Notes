---
tags:
  - cpp/types
  - cpp/casting
  - concept
aliases:
  - implicit conversion
  - automatic type conversion
  - coercion
up: "[[Chapter 10 — Type conversion]]"
related:
  - "[[Standard Conversions]]"
  - "[[Numeric Promotion]]"
  - "[[Numeric Conversions]]"
  - "[[Narrowing Conversions]]"
  - "[[Arithmetic Conversions]]"
  - "[[Implicit Conversion]]"
  - "[[static_cast]]"
---

# Implicit Type Conversion

**Implicit type conversion** occurs when the compiler automatically converts an expression of one type to another type required by the context — no cast syntax is written.

## When it occurs

- Initializing a variable with a value of a different type
- Passing an argument to a function expecting a different type
- Returning a value whose type differs from the declared return type
- Using mixed types in arithmetic expressions (see [[Arithmetic Conversions]])

## Standard conversions

The compiler follows the [[Standard Conversions]] to decide how to convert. The main categories:

| Category | Meaning |
| --- | --- |
| Numeric promotions | Small integral types → `int`/`unsigned int`; `float` → `double` |
| Numeric conversions | Other integral and floating-point conversions (not promotions) |
| Qualification conversions | Add or remove `const`/`volatile` |
| Value transformations | Change the value category of an expression |
| Pointer conversions | `std::nullptr` → pointer types, pointer → pointer |

## Conversion failure

If the compiler cannot find an acceptable conversion, compilation fails with an error.

## Risks

Some implicit conversions silently lose data ([[Narrowing Conversions]]). Use [[List Initialization|brace initialization]] to turn these into compile errors, or [[static_cast]] when an explicit intentional conversion is needed.

The Ch4 note [[Implicit Conversion]] covers the basics; this chapter provides the full categorization and rules.

> Full coverage: [[Chapter 10 — Type conversion]] → Implicit Type Conversion
