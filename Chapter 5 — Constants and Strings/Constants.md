---
tags:
  - cpp/constants
  - cpp/const
  - concept
  - syntax
aliases:
  - const
  - named constant
  - symbolic constant
  - literal constant
  - type qualifier
  - cv-qualifier
  - volatile
up: "[[Chapter 5 — Constants and Strings]]"
related:
  - "[[constexpr]]"
  - "[[Initialization]]"
  - "[[Preprocessing]]"
  - "[[Literals]]"
  - "[[Compile-time Evaluation]]"
---

# Constants

A **constant** is a value that cannot change after initialization.

## Named Constants

Three ways to define a named (symbolic) constant in C++:

1. **`const` variable** *(preferred)*
2. Object-like macro with substitution text (`#define MY_NAME "Alex"`) — avoid; prefer `const`
3. Enumerated constants (`enum`)

**`const` variable rules:**
- Must be initialized at definition — cannot be left uninitialized
- The initializer can be a runtime expression (making it a runtime constant)
- `const` on a value parameter is redundant — don't add it
- `const` on a return-by-value is also redundant — don't add it

```cpp
const int max { 100 };             // compile-time constant
const int input { getUserVal() };  // runtime constant — still valid
```

## Literal Constants

A literal is a fixed value with no identifier: `42`, `3.14`, `"hello"`, `true`.

## Type Qualifiers

Keywords applied to a type that modify its behavior:

| Qualifier | Effect |
|-----------|--------|
| `const` | value cannot change after initialization |
| `volatile` | value may change at any time (hardware-mapped memory, etc.) |

Together called **cv-qualifiers** in the C++ standard.

> Full coverage: [[Chapter 5 — Constants and Strings]] → Constants
