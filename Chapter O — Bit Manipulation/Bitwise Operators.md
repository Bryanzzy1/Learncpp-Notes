---
tags:
  - cpp/bits
  - cpp/operators
  - concept
  - syntax
  - best-practice
aliases:
  - bitwise NOT
  - bitwise AND
  - bitwise OR
  - bitwise XOR
  - bitwise shift
  - operator~
  - operator<<
up: "[[Chapter O — Bit Manipulation]]"
related:
  - "[[Bit Flags]]"
  - "[[Bit Masks]]"
  - "[[Arithmetic Operators]]"
---

# Bitwise Operators

## Type Promotion

Bitwise operators promote operands with narrower integral types (e.g. `uint8_t`) to `int` or `unsigned int` before operating.

- `operator~` and `operator<<` are **width-sensitive** — the result width affects the value produced.
- Leading ones from a bitwise NOT *do* contribute to the interpreted integer value.

**Fix:** `static_cast` the result back to the narrower type:

```cpp
uint8_t b { 0b0000'0101 };
uint8_t result { static_cast<uint8_t>(~b) }; // cast back to 8-bit after NOT
```

> Full coverage: [[Chapter O — Bit Manipulation]] → Bitwise Operators
