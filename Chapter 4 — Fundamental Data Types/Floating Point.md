---
tags:
  - cpp/types
  - cpp/floats
  - concept
  - syntax
aliases:
  - float
  - double
  - floating point
  - precision
  - IEEE 754
  - NaN
  - Inf
up: "[[Chapter 4 — Fundamental Data Types]]"
related:
  - "[[Integers]]"
  - "[[IO]]"
  - "[[Fundamental Data Types]]"
  - "[[Literals]]"
  - "[[Output Manipulators]]"
---

# Floating Point

C++ provides three floating-point types:

| Type | Typical size | Significant digits |
|------|-------------|-------------------|
| `float` | 4 bytes | ~6–7 |
| `double` | 8 bytes | ~15–16 |
| `long double` | 8–16 bytes | ≥ 15 |

- **Prefer `double` over `float`** — `float`'s limited precision frequently causes inaccuracies.
- Match literal type to variable type: use `4.1f` for `float`, `4.1` (no suffix) for `double`.

## Precision

The number of significant digits a type can represent without information loss.

Use [[Output Manipulators|`std::setprecision()`]] from `<iomanip>` to control how many digits `std::cout` displays:

```cpp
#include <iomanip>
std::cout << std::setprecision(16) << 3.14159265358979;
```

## IEEE 754 Special Values

- **`Inf`** — positive or negative infinity (e.g., divide a non-zero float by 0.0)
- **`NaN`** — "Not a Number" (e.g., `0.0 / 0.0`)
- **Signed zero** — `+0.0` and `-0.0` are distinct but compare equal

> Full coverage: [[Chapter 4 — Fundamental Data Types]] → Floats
