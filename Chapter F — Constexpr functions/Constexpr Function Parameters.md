---
tags:
  - cpp/constexpr
  - cpp/functions
  - concept
  - syntax
  - subnode
aliases:
  - constexpr function parameters
  - consteval function parameters
  - parameters not constexpr
up: "[[Constexpr Function Rules]]"
related:
  - "[[Constexpr Function Rules]]"
  - "[[constexpr]]"
  - "[[Consteval]]"
  - "[[Pass by Value]]"
---

# Constexpr Function Parameters

**Parameters of constexpr and consteval functions are not `constexpr`**, even when the function is called with constant-expression arguments.

```cpp
constexpr int square(int x)
{
    constexpr int y { x }; // error: x is not a constant expression inside this function
    return x * x;
}
```

## Why

A parameter's value is determined at the **call site** — the function body has no way to know at definition time whether it will receive a compile-time constant or a runtime value. The parameter is therefore treated as a regular (non-const) variable inside the function body.

This mirrors the rule for [[constexpr]] variables in general: a value is only `constexpr` when the compiler can prove its value is fixed at compile time.

## Contrast with non-type template parameters

If you genuinely need a compile-time constant as a "parameter," use a [[Non-type Template Parameters|non-type template parameter]] instead, which is always a `constexpr` value:

```cpp
template <int N>
constexpr int square() { return N * N; }

constexpr int s { square<4>() }; // N = 4 is a true compile-time constant
```

> Full coverage: [[Chapter F — Constexpr functions]] → Constexpr Function Rules
