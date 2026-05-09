---
tags:
  - cpp/testing
  - cpp/constexpr
  - cpp/error-handling
  - concept
  - syntax
  - subnode
aliases:
  - static assert
  - compile-time assertion
  - static_assert()
up: "[[Assertions]]"
related:
  - "[[Assertions]]"
  - "[[constexpr]]"
  - "[[Compile-time Evaluation]]"
  - "[[Chapter 9 — Error handling]]"
---

# static_assert

A **`static_assert`** is an assertion evaluated entirely at **compile time**. A failing `static_assert` produces a compiler error — the program never runs at all.

```cpp
static_assert(condition, "optional diagnostic message");
```

## Requirements

The condition must be a **constant expression** — see [[constexpr]] and [[Compile-time Evaluation]]. It cannot depend on runtime values.

```cpp
constexpr int x { 5 };
static_assert(x > 0, "x must be positive"); // OK: x is constexpr

int y { 5 };
// static_assert(y > 0); // Error: y is not a constant expression
```

## Prefer static_assert over assert()

| | `assert()` | `static_assert` |
|--|-----------|----------------|
| Evaluated at | Runtime | Compile time |
| Failure result | Program abort | Compiler error |
| Overhead in release builds | Zero (NDEBUG) | Zero (compile-time only) |
| Can use runtime values | Yes | No |

Favor `static_assert` over `assert()` whenever the condition is knowable at compile time — the error is caught earlier and there is no runtime cost.

> Full coverage: [[Chapter 9 — Error handling]] → static_assert
