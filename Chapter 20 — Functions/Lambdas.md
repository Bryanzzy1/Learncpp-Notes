---
tags:
  - cpp/functions
  - cpp/lambdas
  - concept
  - syntax
  - best-practice
aliases:
  - lambda expression
  - lambda
  - closure
  - anonymous function
  - functor
  - function literal
  - generic lambda
  - constexpr lambda
up: "[[Chapter 20 — Functions]]"
related:
  - "[[Lambda Captures]]"
  - "[[Function Pointers]]"
  - "[[Function Templates]]"
  - "[[Type Deduction]]"
  - "[[Default Arguments]]"
  - "[[Functions]]"
  - "[[Chapter 20 — Functions]]"
---

# Lambdas

A **lambda expression** (also called a **lambda**, **closure**, or **anonymous function**) defines an unnamed function inline, close to where it is used. Lambdas avoid namespace pollution and keep related logic together.

## Syntax

```cpp
[ captureClause ] ( parameters ) -> returnType
{
    statements;
}
```

- **Capture clause** — required (may be empty `[]`). Controls access to surrounding-scope variables; see [[Lambda Captures]].
- **Parameters** — optional; omit entirely if there are none and no return type is specified.
- **Return type** — optional; defaults to `auto` (type deduction from `return` statements).

```cpp
// Minimal lambda
[] {};

// Lambda passed directly as argument
auto found{ std::find_if(arr.begin(), arr.end(),
    [](std::string_view str) {
        return str.find("nut") != std::string_view::npos;
    }) };
```

## Lambdas are functors, not functions

Lambdas are syntactic sugar for **functors** — compiler-generated objects with an overloaded `operator()`. This is why C++ can support nested lambdas even though nested functions are not allowed.

## Storing lambdas

Store a lambda in a variable to reuse it or give it a meaningful name:

```cpp
auto isEven = [](int n) { return n % 2 == 0; };
std::count_if(v.begin(), v.end(), isEven);
```

The only portable way to store a lambda with its exact type is `auto`. For lambdas without captures, a raw [[Function Pointers|function pointer]] works. For general callables, use `std::function<Ret(Args...)>`.

## Generic lambdas (C++14)

Use `auto` parameters to make a lambda work with multiple types — the compiler generates a templated `operator()`:

```cpp
auto add = [](auto x, auto y) { return x + y; };
add(1, 2);     // int
add(1.5, 2.5); // double
```

Each distinct type combination instantiates a separate `operator()` — similar to [[Function Templates]].

Note: generic lambdas with `static` local variables do **not** share those variables across different instantiated types.

## Constexpr lambdas (C++17)

A lambda is implicitly `constexpr` when:
- It has no captures (or all captures are `constexpr`), **and**
- All functions it calls are `constexpr`.

Many standard library algorithms and math functions became `constexpr` only in C++20/23.

## Return type deduction

When the return type is omitted, it is deduced from `return` statements. **All return statements must return the same type**; a mismatch is a compile error.

Use a trailing return type to be explicit when types differ or when the deduced type isn't obvious:

```cpp
[](int x) -> double { return x; }
```

## Standard library function objects

`<functional>` provides pre-built functors for common comparisons:

```cpp
#include <functional>
std::sort(arr.begin(), arr.end(), std::greater{}); // descending sort
```

> Full coverage: [[Chapter 20 — Functions]] → Lambdas
