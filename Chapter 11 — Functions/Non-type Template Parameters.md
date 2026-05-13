---
tags:
  - cpp/templates
  - cpp/functions
  - concept
  - syntax
  - subnode
aliases:
  - non-type template parameter
  - template value parameter
  - NTTP
up: "[[Function Templates]]"
related:
  - "[[Function Templates]]"
  - "[[Numeric Promotion]]"
  - "[[Numeric Conversions]]"
  - "[[constexpr]]"
  - "[[Chapter 11 — Functions]]"
---

# Non-type Template Parameters

A **non-type template parameter** (NTTP) is a template parameter with a **fixed type** that accepts a `constexpr` value rather than a type:

```cpp
template <int N>
void print()
{
    std::cout << N << '\n';
}

print<5>(); // N = 5
```

The value `N` is resolved at compile time and can be used anywhere a constant expression is required.

## Use cases

NTTPs are primarily useful when you need to pass a [[constexpr]] value into a function or class template so it can appear in a constant-expression context — e.g., array sizes, compile-time loop bounds, or `static_assert` arguments.

## Allowed implicit conversions

When passing a template argument to an NTTP, only constexpr-safe conversions are permitted:

| Conversion type | Example |
| --- | --- |
| Integral promotion | `char` → `int` |
| Integral conversion | `int` → `char`, `char` → `long` |
| User-defined conversion | program-defined class → `int` |
| Lvalue-to-rvalue | variable `x` → value of `x` |

Standard [[Numeric Conversions]] and [[Numeric Promotion|promotions]] apply here, but only those that are constexpr-compatible.

## `auto` NTTPs (C++17)

Use `auto` to let the compiler deduce the NTTP type from the argument:

```cpp
template <auto N>
void print()
{
    std::cout << N << '\n';
}

print<5>();    // N deduced as int
print<'A'>();  // N deduced as char
```

> Full coverage: [[Chapter 11 — Functions]] → Function Templates
