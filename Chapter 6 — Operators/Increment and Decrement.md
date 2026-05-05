---
tags:
  - cpp/operators
  - cpp/increment
  - concept
  - syntax
  - best-practice
aliases:
  - pre-increment
  - post-increment
  - pre-decrement
  - post-decrement
  - side effect
up: "[[Chapter 6 — Operators]]"
related:
  - "[[Side Effects]]"
  - "[[Undefined Behavior]]"
---

# Increment and Decrement

| Operator | Symbol | Form | Operation |
|----------|--------|------|-----------|
| Prefix increment | `++` | `++x` | Increment x, then return x |
| Prefix decrement | `--` | `--x` | Decrement x, then return x |
| Postfix increment | `++` | `x++` | Copy x, then increment x, then return the copy |
| Postfix decrement | `--` | `x--` | Copy x, then decrement x, then return the copy |

## Side Effects

A function or expression has a **side effect** if it has an observable effect beyond producing a return value. Increment/decrement operators are side-effectful.

## Unspecified Behavior

- The C++ standard does **not** define the order in which function arguments are evaluated.
- C++ does **not** specify when the side effects of operators must be applied.
- `x + ++x` is **unspecified behavior** — compilers differ on result (e.g. `2 + 2` in GCC vs `1 + 2` in Clang when `x = 1`).

> Full coverage: [[Chapter 6 — Operators]] → Increment and Decrement
