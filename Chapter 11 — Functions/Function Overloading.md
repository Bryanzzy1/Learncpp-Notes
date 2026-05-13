---
tags:
  - cpp/functions
  - concept
  - syntax
aliases:
  - function overloading
  - overloaded function
  - overload
  - type signature
  - name mangling
  - function signature
up: "[[Chapter 11 — Functions]]"
related:
  - "[[Overload Resolution]]"
  - "[[Deleted Functions]]"
  - "[[Default Arguments]]"
  - "[[Function Templates]]"
  - "[[Functions]]"
  - "[[Type Aliases]]"
  - "[[Numeric Promotion]]"
  - "[[Numeric Conversions]]"
---

# Function Overloading

**Function overloading** lets you define multiple functions with the same name in the same scope, provided each can be differentiated by the compiler — typically by having different parameter types or counts.

```cpp
int add(int x, int y)       { return x + y; }
double add(double x, double y) { return x + y; }
```

Each identically-named function is called an **overloaded function** (or **overload**).

## Differentiation rules

| Property | Used for differentiation | Notes |
| --- | --- | --- |
| Number of parameters | Yes | |
| Type of parameters | Yes | Excludes typedefs/type aliases and top-level `const` on value parameters. Includes ellipses. |
| Return type | **No** | |

Because [[Type Aliases]] are not distinct types, overloads that differ only by a typedef or alias are **not** distinct.

For member functions, `const`/`volatile` qualifiers and ref-qualifiers are also used for differentiation.

## Type signature

A function's **type signature** (or just **signature**) is the set of properties used for differentiation: function name, number of parameters, parameter types, and function-level qualifiers. The return type is **not** part of the signature.

## Name mangling

When the compiler compiles a function it performs **name mangling** — the compiled symbol name is derived from the signature so the linker can distinguish overloads:

- `int fcn()` → `__fcn_v`
- `int fcn(int)` → `__fcn_i`

## Compilation requirements

For a program using overloaded functions to compile:
1. Each overloaded function must be differentiable from the others.
2. Every call to an overloaded function must resolve to exactly one overload.

When the compiler cannot find a unique best match it issues an **ambiguous function call** error (see [[Overload Resolution]]).

> Full coverage: [[Chapter 11 — Functions]] → Function Overloading
