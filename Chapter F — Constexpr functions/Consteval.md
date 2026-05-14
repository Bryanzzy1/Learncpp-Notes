---
tags:
  - cpp/constexpr
  - cpp/functions
  - concept
  - syntax
aliases:
  - consteval function
  - immediate function
  - is_constant_evaluated
  - if consteval
  - constant-evaluated context
  - constant context
up: "[[Chapter F — Constexpr functions]]"
related:
  - "[[Constexpr Function Rules]]"
  - "[[constexpr]]"
  - "[[Compile-time Evaluation]]"
  - "[[Functions]]"
---

# Consteval

Introduced in **C++20**, `consteval` declares a function that **must** evaluate at compile time — if it cannot, the program fails to compile.

```cpp
consteval int square(int x) { return x * x; }

constexpr int s { square(4) }; // OK: compile-time evaluation
int n { getUserInput() };
int r { square(n) };            // compile error: n is not a constant expression
```

## Immediate functions

A `consteval` function is called an **immediate function**. Every call to an immediate function must produce a constant expression; there is no runtime fallback as there is with [[Constexpr Function Rules|`constexpr` functions]].

Use `consteval` when a function **must** do something that only makes sense at compile time (e.g., computing a type-level property, enforcing a compile-time invariant).

## `std::is_constant_evaluated()` (C++20)

```cpp
#include <type_traits>

constexpr int compute(int x)
{
    if (std::is_constant_evaluated())
        return /* compile-time path */;
    else
        return /* runtime path */;
}
```

`std::is_constant_evaluated()` (from `<type_traits>`) returns `true` when the current function is executing in a **constant-evaluated context** — one where a constant expression is required, such as initializing a [[constexpr]] variable. This lets a single `constexpr` function take different code paths at compile time vs. runtime.

## `if consteval` (C++23)

C++23 replaces `if (std::is_constant_evaluated())` with the cleaner:

```cpp
constexpr int compute(int x)
{
    if consteval
    {
        // compile-time path
    }
    else
    {
        // runtime path
    }
}
```

`if consteval` fixes subtle edge cases in the older `std::is_constant_evaluated()` approach and is the preferred form in C++23 and later.

## constexpr vs. consteval

| | `constexpr` | `consteval` |
|---|---|---|
| Compile-time evaluation | When possible / required | Always — compile error otherwise |
| Runtime fallback | Yes | No |
| C++ version | C++11 | C++20 |

> Full coverage: [[Chapter F — Constexpr functions]] → Consteval
