---
tags:
  - cpp/templates
  - cpp/types
  - concept
  - syntax
  - subnode
aliases:
  - CTAD
  - class template argument deduction
  - deduction guide
  - template argument deduction guide
up: "[[Class Templates]]"
related:
  - "[[Class Templates]]"
  - "[[Alias Templates]]"
  - "[[Function Templates]]"
  - "[[Type Deduction]]"
  - "[[Initialization]]"
---

# CTAD (Class Template Argument Deduction)

**CTAD** (Class Template Argument Deduction, C++17) lets the compiler deduce template arguments for a class template from the initializer, removing the need to spell out the types explicitly.

```cpp
std::pair p1 { 3.4f, 5.6f }; // deduced: pair<float, float>
std::pair p2 { 1u, 2u };     // deduced: pair<unsigned int, unsigned int>
```

CTAD is only attempted when **no template argument list is present**.

## Limitations

| Context | CTAD works? |
|---|---|
| Variable initialization | Yes |
| Function parameters | **No** — must specify explicitly |
| Non-static member initialization | **No** — must specify explicitly |

## Deduction guides

A **deduction guide** tells the compiler how to map constructor arguments to template arguments. Required in C++17 for aggregates (structs with no user-provided constructor):

```cpp
template <typename T, typename U>
Pair(T, U) -> Pair<T, U>;
// Now: Pair p { 1, 2 }; deduces Pair<int, int>
```

- **C++20 aggregates**: the compiler auto-generates deduction guides — no manual guide needed.
- **Non-aggregates** (have constructors): the constructor itself serves as the deduction guide in C++17.

## Default template arguments

Template type parameters can have defaults, used when the argument is not deduced or specified:

```cpp
template <typename T = int, typename U = int>
struct Pair { T first{}; U second{}; };
```

> Full coverage: [[Chapter 13 — Compound Types]] → CTAD
