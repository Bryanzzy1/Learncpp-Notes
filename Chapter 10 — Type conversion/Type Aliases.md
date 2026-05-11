---
tags:
  - cpp/types
  - cpp/syntax
  - concept
  - syntax
  - best-practice
aliases:
  - type alias
  - using declaration
  - typedef
  - strong typedef
up: "[[Chapter 10 — Type conversion]]"
related:
  - "[[Translation Unit]]"
  - "[[Header Files]]"
---

# Type Aliases

A **type alias** creates an alternative name for an existing type. The modern C++ syntax uses the `using` keyword.

```cpp
using Distance = double;
Distance milesToDestination{ 3.4 }; // Distance is double
```

Type aliases can be templated. Name them starting with a capital letter.

## Typedef (legacy)

```cpp
typedef long Miles;   // old syntax
using Miles = long;   // equivalent, preferred
```

Prefer `using` over `typedef` — it is clearer and supports templates.

## Aliases are not distinct types

A type alias is **not type-safe**: the compiler treats the alias and the original type as interchangeable. Mixing `Distance` and `double` raises no error.

Some languages provide **strong typedefs** that create genuinely distinct types; C++ (as of C++20) does not support this directly.

## Scope

Type aliases follow the same scoping rules as variables: block scope inside a block, global scope when defined at namespace level. Put shared aliases in a [[Header Files|header file]] to make them available across [[Translation Unit|translation units]].

## Why use type aliases?

- **Platform independence**: `using int32 = int;` abstracts over platform-specific width.
- **Readability**: complex template types become a short, readable name.
- **Semantic documentation**: `Distance` is more meaningful than `double`.
- **Easier maintenance**: change the underlying type in one place.

> Full coverage: [[Chapter 10 — Type conversion]] → Type Aliases
