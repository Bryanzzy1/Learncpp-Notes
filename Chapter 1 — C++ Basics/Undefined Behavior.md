---
tags:
  - cpp/basics
  - concept
  - best-practice
aliases:
  - undefined behavior
  - UB
  - implementation-defined behavior
  - unspecified behavior
up: "[[Chapter 1 — C++ Basics]]"
related:
  - "[[Initialization]]"
  - "[[Integers]]"
  - "[[Memory Model]]"
---

# Undefined Behavior

C++ defines three categories of behavior that programs should avoid:

| Category | Meaning | Example |
|----------|---------|---------|
| **Undefined behavior (UB)** | The standard places no requirements on what happens — the program may crash, produce garbage, or appear to work | Reading an uninitialized variable; signed integer overflow |
| **Implementation-defined behavior** | The behavior is valid but the exact result is chosen by the compiler/platform | Size of `int`; result of right-shifting a negative signed integer |
| **Unspecified behavior** | Multiple valid behaviors exist; the implementation picks one but need not document it | Order of evaluation of function arguments |

## Common Sources of UB

- Using an [[Initialization|uninitialized variable]]
- Signed [[Integers|integer overflow]]
- Out-of-bounds array access
- Dereferencing a null or dangling pointer
- Modifying a `const` object

## Why UB Is Dangerous

The compiler is allowed to **assume UB never occurs**. This lets it optimize aggressively, but means that code containing UB can produce entirely unexpected results — including results that appear correct in debug builds and fail in release builds.

> **Always avoid UB.** Prefer [[List Initialization|brace initialization]], use [[static_cast]] for explicit conversions, and enable compiler warnings.

> Full coverage: [[Chapter 1 — C++ Basics]] → Implementation-defined and unspecified behavior
