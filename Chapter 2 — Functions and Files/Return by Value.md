---
tags:
  - cpp/functions
  - cpp/optimization
  - concept
  - syntax
  - subnode
aliases:
  - return value
  - copy elision
  - NRVO
  - named return value optimization
up: "[[Functions]]"
related:
  - "[[Pass by Value]]"
  - "[[Memory Model]]"
  - "[[constexpr Functions]]"
---

# Return by Value

When a function returns, it produces a **temporary object** holding a copy of the return value, which initializes the caller's variable.

```cpp
int add(int x, int y) {
    return x + y;            // creates a temporary holding the result
}
int result { add(3, 4) };   // result is initialized from the temporary
```

## Copy Elision (C++17)

In modern C++ — especially C++17 — the compiler often **elides** (skips) the temporary entirely and constructs the result directly into the caller's variable. This optimization is called **NRVO** (Named Return Value Optimization) and is guaranteed in many cases by the standard.

The practical effect: returning by value is usually free at runtime in optimized builds.

## constexpr Functions

When all arguments are compile-time constants, a [[constexpr Functions|constexpr function]] is evaluated entirely at compile time — no temporary is created at runtime at all.

> Full coverage: [[Chapter 2 — Functions and Files]] → Return by value
