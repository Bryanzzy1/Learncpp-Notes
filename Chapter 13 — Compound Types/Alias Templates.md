---
tags:
  - cpp/templates
  - cpp/types
  - concept
  - syntax
  - subnode
aliases:
  - alias template
  - template alias
up: "[[Class Templates]]"
related:
  - "[[Class Templates]]"
  - "[[CTAD]]"
  - "[[Type Aliases]]"
  - "[[Translation Unit]]"
  - "[[Header Files]]"
---

# Alias Templates

An **alias template** is a template that instantiates [[Type Aliases|type aliases]]. The user still supplies the type argument each time they use it.

## Plain type alias for a fully-specified class template

When all template arguments are known, a normal `using` alias works:

```cpp
using Point = Pair<int>; // Pair<int> fully specified
Point p { 1, 2 };        // compiler treats Point as Pair<int>
```

## Alias template (user supplies the type)

When you want the alias itself to remain generic:

```cpp
// Alias templates must be defined in global scope
template <typename T>
using Coord = Pair<T>; // Coord<T> is an alias for Pair<T>

Coord<int> c1 { 1, 2 };
Coord<double> c2 { 1.5, 2.5 };
```

## CTAD with alias templates (C++20)

In C++20, alias template deduction allows the compiler to deduce `T` in cases where [[CTAD]] would normally work:

```cpp
Coord c { 1, 2 }; // C++20: T deduced as int
```

## Scope

Like all template definitions, alias templates must be in global (namespace) scope — not inside a function. Put them in [[Header Files|header files]] for use across [[Translation Unit|translation units]].

> Full coverage: [[Chapter 13 — Compound Types]] → Alias Templates
