---
tags:
  - cpp/operators
  - cpp/comparison
  - concept
  - syntax
  - best-practice
aliases:
  - comparison operators
  - "=="
  - "!="
  - relational operators
up: "[[Chapter 6 — Operators]]"
related:
  - "[[Floating Point]]"
  - "[[Conditional Operator]]"
---

# Relational Operators

Standard comparison operators: `==`, `!=`, `<`, `>`, `<=`, `>=`

## Floating Point Comparison

Avoid `==` and `!=` with floating-point values that have been **calculated** — rounding errors make exact equality unreliable.

```cpp
double a { 0.1 + 0.2 };
if (a == 0.3)  // likely false due to floating-point rounding!
```

**Exception:** safe to compare a floating-point literal directly with a variable of the same type initialized from the same literal.

> Full coverage: [[Chapter 6 — Operators]] → Relational Operators
