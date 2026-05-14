---
tags:
  - cpp/constexpr
  - cpp/functions
  - concept
  - syntax
  - subnode
aliases:
  - constexpr non-const locals
  - constexpr local variables
  - constexpr function arguments
up: "[[Constexpr Function Rules]]"
related:
  - "[[Constexpr Function Rules]]"
  - "[[Constexpr Function Parameters]]"
  - "[[Consteval]]"
  - "[[constexpr]]"
---

# Constexpr Function Capabilities

Two capabilities of `constexpr` and `consteval` functions that are easy to misunderstand:

## Non-const local variables are allowed

A constexpr function can declare and modify **non-const local variables** inside its body. The `constexpr` qualifier applies to the *function's result*, not to every variable inside it.

```cpp
constexpr int sumTo(int n)
{
    int total { 0 }; // non-const local — fine inside a constexpr function
    for (int i { 1 }; i <= n; ++i)
        total += i;
    return total;
}

constexpr int s { sumTo(5) }; // evaluates at compile-time: s = 15
```

## Function parameters can be forwarded to other constexpr calls

Even though [[Constexpr Function Parameters|function parameters are not themselves `constexpr`]], they **can** be passed as arguments to other `constexpr` function calls within the body. When the outer function is resolved at compile time, the compiler knows the parameter values and can propagate them into the inner call.

```cpp
constexpr int goo(int c)
{
    return c;
}

constexpr int foo(int b) // b is not constexpr within foo()
{
    return goo(b);       // if foo() is resolved at compile-time, goo(b) can be too
}
```

The key insight: compile-time resolution flows **top-down**. If the outermost call is in a constant-expression context, the compiler evaluates the entire call chain at compile time, treating all parameters as known values for that evaluation.

> Full coverage: [[Chapter F — Constexpr functions]] → Constexpr Function Rules
