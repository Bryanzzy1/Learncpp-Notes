---
tags:
  - cpp/types
  - cpp/casting
  - best-practice
  - concept
aliases:
  - narrowing conversion
  - narrowing
up: "[[Chapter 10 — Type conversion]]"
related:
  - "[[Numeric Conversions]]"
  - "[[Explicit Type Conversion]]"
  - "[[List Initialization]]"
  - "[[static_cast]]"
  - "[[Implicit Conversion]]"
  - "[[constexpr]]"
---

# Narrowing Conversions

A **narrowing conversion** is an implicit numeric conversion that may not preserve the source value. The C++ standard defines the following as narrowing:

- Floating-point type → integral type
- Floating-point type → a narrower or lower-ranked floating-point type (unless the value is `constexpr` and in range of the destination)
- Integral type → floating-point type (unless the value is `constexpr` and exactly representable in the destination)
- Integral type → another integral type that cannot represent all values of the original type (unless the value is `constexpr` and exactly representable). Covers both wider-to-narrower and signed/unsigned conversions.

## Brace initialization disallows narrowing

[[List Initialization|Brace initialization]] (`{}`) treats any narrowing conversion as a compile error:

```cpp
int x { 3.5 };   // compile error: narrowing (double → int)
int y = 3.5;      // compiles with warning, but truncates to 3
```

Prefer brace initialization to catch narrowing at compile time.

## constexpr initializers are exempt

When the initializer is a `constexpr` value that fits in the destination type, brace initialization allows it — the compiler can verify safety at compile time:

```cpp
constexpr int n{ 5 };
double d { n };       // OK: n is constexpr and fits in double
short s { 5 };        // OK: literal 5 fits in short
unsigned int u { 5 }; // OK: no suffix needed
float f { 1.5 };      // OK: no suffix needed
```

See [[constexpr]] for rules on constant expressions.

## Making narrowing explicit

If a narrowing conversion is genuinely needed, use [[static_cast]] to signal intent:

```cpp
double x { 3.5 };
int y { static_cast<int>(x) }; // explicit truncation — no compile error
```

> Full coverage: [[Chapter 10 — Type conversion]] → Narrowing Conversions
