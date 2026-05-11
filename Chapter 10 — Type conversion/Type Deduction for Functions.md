---
tags:
  - cpp/types
  - cpp/functions
  - concept
  - syntax
  - subnode
aliases:
  - auto return type
  - trailing return type
  - trailing return syntax
up: "[[Type Deduction]]"
related:
  - "[[Type Deduction]]"
  - "[[Functions]]"
  - "[[Header Files]]"
  - "[[Chapter 10 — Type conversion]]"
---

# Type Deduction for Functions

The `auto` keyword can be used as a function return type, causing the compiler to deduce the return type from the `return` statement.

```cpp
auto add(int x, int y)
{
    return x + y; // return type deduced as int
}
```

## Constraints

- All `return` statements in the function must return values of the **same type**; mixing types is a compile error.
- The function must be **fully defined** before it can be called — a forward declaration alone is not sufficient.

## Downsides

Because forward declarations are insufficient, auto-return functions cannot be declared in a [[Header Files|header file]] without exposing the full definition. Callers must see the definition. Prefer explicit return types except when the return type is unimportant, difficult to express, or fragile.

## Trailing return syntax

The `auto` keyword also enables **trailing return syntax**, where the return type is written after the parameter list:

```cpp
auto add(int x, int y) -> int
{
    return x + y;
}
```

Useful for complex types or template functions where the return type depends on parameters.

## auto for function parameters

`auto` cannot be used for function parameter types in C++17 and earlier. In C++20, it triggers **function templates** rather than simple type deduction. See [[Functions]] for the standard parameter passing rules.

> Full coverage: [[Chapter 10 — Type conversion]] → Type Deduction for Functions
