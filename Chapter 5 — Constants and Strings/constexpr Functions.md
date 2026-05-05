---
tags:
  - cpp/constexpr
  - cpp/functions
  - concept
  - syntax
  - subnode
aliases:
  - constexpr function
  - compile-time function
up: "[[constexpr]]"
related:
  - "[[constexpr]]"
  - "[[Compile-time Evaluation]]"
  - "[[Return by Value]]"
  - "[[Functions]]"
---

# constexpr Functions

A **constexpr function** can be evaluated at compile time when all its arguments are compile-time constants — and at runtime otherwise. Write it once, get compile-time evaluation for free.

```cpp
constexpr int square(int x) {
    return x * x;
}

constexpr int s1 { square(4) };  // compile-time: s1 = 16, no runtime cost
int n { getUserInput() };
int s2 { square(n) };            // runtime: n is not constexpr
```

## Rules

- All arguments must be compile-time constant expressions for compile-time evaluation to occur
- When called with non-constant arguments, behaves exactly like a normal function
- Function **parameters** cannot be declared `constexpr` — their value is determined at the call site, not at compile time
- The return value must be a literal type (no dynamic allocation, no virtual functions)

## Why It Matters

Constexpr functions enable **compile-time programming**: logic that would otherwise run at startup can be moved entirely into the compilation step, producing faster and leaner binaries.

> Full coverage: [[Chapter 5 — Constants and Strings]] → Constant expressions
