---
tags:
  - cpp/operators
  - concept
  - syntax
aliases:
  - comma operator
up: "[[Chapter 6 — Operators]]"
related:
  - "[[Conditional Operator]]"
  - "[[Increment and Decrement]]"
---

# Comma Operator

Evaluates the left operand, then the right operand, and returns the value of the right operand.

```cpp
std::cout << (++x, ++y) << '\n'; // increments both x and y, outputs the value of y
```

Rarely used in modern C++; most common in `for`-loop expressions.

> Full coverage: [[Chapter 6 — Operators]] → Comma Operator
