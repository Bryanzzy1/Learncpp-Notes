---
tags:
  - cpp/operators
  - concept
  - syntax
  - best-practice
aliases:
  - ternary operator
  - "?:"
up: "[[Chapter 6 — Operators]]"
related:
  - "[[Relational Operators]]"
  - "[[Comma Operator]]"
---

# Conditional Operator

The **conditional (ternary) operator** `?:` is C++'s only ternary operator:

```
c ? x : y
```

- If `c` is `true`, evaluates and returns `x`; otherwise evaluates and returns `y`

## Rules

- **Parenthesize** the entire conditional expression when used inside a larger expression.
- The second and third operands must have matching types, or one must be convertible to the other (or be a throw expression).

```cpp
int x { (a > b) ? a : b };        // parenthesize the whole conditional
std::cout << ((a > b) ? "a" : "b");
```

> Full coverage: [[Chapter 6 — Operators]] → Conditional Operator
