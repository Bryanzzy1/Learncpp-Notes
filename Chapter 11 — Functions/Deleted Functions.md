---
tags:
  - cpp/functions
  - concept
  - syntax
  - subnode
aliases:
  - = delete
  - deleted function
  - delete specifier
up: "[[Function Overloading]]"
related:
  - "[[Function Overloading]]"
  - "[[Overload Resolution]]"
  - "[[Function Templates]]"
  - "[[Chapter 11 — Functions]]"
---

# Deleted Functions

The `= delete` specifier marks a function as **forbidden**. Any call that resolves to a deleted function produces a compile error.

```cpp
void printInt(int x)   { std::cout << x << '\n'; }
void printInt(char)  = delete;
void printInt(bool)  = delete;

printInt(97);    // ok — matches printInt(int)
printInt('a');   // error: printInt(char) is deleted
printInt(true);  // error: printInt(bool) is deleted
printInt(5.0);   // error: ambiguous match (double → int or deleted overloads)
```

## Key rule

`= delete` means **"I forbid this"**, not "this doesn't exist." Deleted functions participate in **all stages** of [[Overload Resolution]], including the exact-match stage. If overload resolution selects a deleted function, compilation fails.

## Deleting all non-matching overloads via a template

A deleted function template catches every type not explicitly handled:

```cpp
template <typename T>
void printInt(T x) = delete; // any type other than int → compile error
```

Because [[Function Templates|function templates]] are more general, the explicit `printInt(int)` overload takes precedence for `int` arguments; all other types match the deleted template and fail to compile.

> Full coverage: [[Chapter 11 — Functions]] → Function Overloading
