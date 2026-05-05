---
tags:
  - cpp/casting
  - cpp/types
  - concept
  - subnode
aliases:
  - implicit type conversion
  - automatic conversion
  - coercion
up: "[[static_cast]]"
related:
  - "[[static_cast]]"
  - "[[List Initialization]]"
  - "[[Integers]]"
  - "[[Floating Point]]"
---

# Implicit Conversion

**Implicit type conversion** (also called coercion) happens when the compiler automatically converts a value from one type to another — no cast syntax is written.

```cpp
double d { 3 };      // int 3 → double 3.0 (no data loss — safe)
int i = 3.9;         // double 3.9 → int 3 (truncates — data lost, compiler may warn)
```

## When It Occurs

- Arithmetic on mixed types: `int + double` → compiler promotes `int` to `double` before the operation
- Passing a value to a function expecting a different type
- Returning a value whose type differs from the declared return type

## The Risk

Some implicit conversions silently lose information (narrowing). The compiler may or may not warn. Use [[List Initialization|brace initialization]] to make these compile errors instead:

```cpp
int bad  = 3.9;     // silently truncates: bad = 3
int good { 3.9 };   // compile error: narrowing conversion not allowed
```

## Explicit Alternative

Use [[static_cast]] when you intend the conversion and want to be explicit:
```cpp
int result { static_cast<int>(3.9) };  // intentional truncation, clearly marked
```

> Full coverage: [[Chapter 4 — Fundamental Data Types]] → Type Casting
