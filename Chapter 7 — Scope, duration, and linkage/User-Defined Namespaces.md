---
tags:
  - cpp/namespaces
  - concept
  - syntax
  - best-practice
aliases:
  - program-defined namespaces
  - user-defined namespaces
  - namespace alias
  - nested namespaces
up: "[[Chapter 7 — Scope, duration, and linkage]]"
related:
  - "[[Using Declarations and Directives]]"
  - "[[Unnamed and Inline Namespaces]]"
  - "[[Internal Linkage]]"
---

# User-Defined Namespaces

Namespaces you create are called **user-defined namespaces** (more precisely, **program-defined namespaces**).

## Syntax

```cpp
namespace NamespaceIdentifier
{
    // content of namespace here
}
```

- Start namespace names with a **capital letter**.
- Namespace blocks can be declared in multiple locations (across files or within the same file); all declarations become part of the same namespace.

## Scope resolution

Use `::` to call identifiers within a namespace:

```cpp
NamespaceIdentifier::someFunction();
```

If no scope resolution is provided inside a namespace, the compiler searches:
1. The current namespace
2. Each containing namespace outward
3. The global namespace (last)

## Nested namespaces

Namespaces can be nested inside other namespaces:

```cpp
namespace Outer::Inner { ... }   // C++17 shorthand
```

## Namespace aliases

Shorten a long or nested namespace name:

```cpp
namespace Active = Foo::Goo;  // Active now refers to Foo::Goo
```

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → User-Defined Namespaces
