---
tags:
  - cpp/optimization
  - cpp/constexpr
  - concept
aliases:
  - as-if rule
  - constant folding
  - constant propagation
  - dead code elimination
  - compile-time constant
  - runtime constant
  - compile-time programming
up: "[[Chapter 5 — Constants and Strings]]"
related:
  - "[[constexpr]]"
  - "[[Constants]]"
  - "[[Debugging]]"
---

# Compile-time Evaluation

Modern compilers evaluate expressions at compile-time wherever possible, reducing work done at runtime.

## As-if Rule

The compiler may transform a program however it likes, provided the **observable behavior** (I/O, volatile reads/writes, etc.) is unchanged. This is the legal foundation for all compile-time optimizations.

## Compile-time Optimizations

- **Constant folding** — the compiler replaces `2 + 3` with `5` in the output
- **Constant propagation** — known constant values are substituted at every use site, eliminating variable reads
- **Dead code elimination** — branches the compiler proves are never taken are removed entirely

Debug builds disable all of these so runtime behavior exactly mirrors the source. See [[Debugging]].

## Compile-time vs. Runtime Constants

| | Compile-time constant | Runtime constant |
|--|----------------------|-----------------|
| Value known at | Compilation | Execution |
| Examples | Literals, `constexpr` vars, `const` vars initialized with compile-time constants | `const int x { getUserInput() }` |
| Can be used in constant expressions? | Yes | No |

> Full coverage: [[Chapter 5 — Constants and Strings]] → The as-if rule and compile-time optimization
