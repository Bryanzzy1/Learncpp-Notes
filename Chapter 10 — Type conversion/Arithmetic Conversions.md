---
tags:
  - cpp/types
  - cpp/operators
  - concept
aliases:
  - arithmetic conversion
  - usual arithmetic conversions
  - common type
  - std::common_type
  - std::common_type_t
up: "[[Chapter 10 — Type conversion]]"
related:
  - "[[Implicit Type Conversion]]"
  - "[[Numeric Promotion]]"
  - "[[Integers]]"
  - "[[Floating Point]]"
---

# Arithmetic Conversions

When a binary operator receives operands of different types, the compiler applies the **usual arithmetic conversions** to bring them to a common type before the operation.

## Rules (applied in order)

1. If one operand is a floating-point type and the other is integral, the integral operand is converted to the floating-point type (no promotion applied first).
2. Otherwise, integral operands are [[Numeric Promotion|numerically promoted]].
3. After promotion, if one operand is signed and the other is unsigned:
   - Unsigned rank ≥ signed rank → signed converts to unsigned.
   - Signed type can represent all values of the unsigned type → unsigned converts to signed.
   - Otherwise → both convert to the unsigned counterpart of the signed type.
4. Otherwise, the lower-ranked operand converts to the type of the higher-ranked operand.

## std::common_type

`std::common_type` (from `<type_traits>`) exposes the result type that the usual arithmetic conversions would produce:

```cpp
#include <type_traits>
std::common_type_t<int, double>        // → double
std::common_type_t<unsigned int, long> // → determined by rules above
```

Useful for templates that need to return the type of a mixed arithmetic expression.

> Full coverage: [[Chapter 10 — Type conversion]] → Arithmetic Conversions
