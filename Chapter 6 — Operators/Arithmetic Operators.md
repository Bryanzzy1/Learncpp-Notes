---
tags:
  - cpp/operators
  - cpp/arithmetic
  - concept
  - syntax
aliases:
  - remainder operator
  - modulo
  - exponent
  - pow
up: "[[Chapter 6 — Operators]]"
related:
  - "[[Increment and Decrement]]"
  - "[[Floating Point]]"
---

# Arithmetic Operators

## Remainder (%)

C++ uses `%` for **remainder** (not modulo — they differ for negative numbers):

- `21 % 4` → `1`
- C++ remainder takes the sign of the dividend

## Exponents

No built-in exponent operator — use `std::pow()` from `<cmath>`:

```cpp
#include <cmath>

double x{ std::pow(3.0, 4.0) }; // 3 to the 4th power = 81
```

> Full coverage: [[Chapter 6 — Operators]] → Arithmetic Operators
