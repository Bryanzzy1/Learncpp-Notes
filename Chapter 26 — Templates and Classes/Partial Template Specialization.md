---
tags:
  - cpp/templates
  - cpp/classes
  - concept
  - syntax
  - subnode
aliases:
  - partial template specialization
  - partial specialization
  - partial class specialization
up: "[[Class Template Specialization]]"
related:
  - "[[Class Template Specialization]]"
  - "[[Partial Template Specialization for Pointers]]"
  - "[[Function Template Specialization]]"
  - "[[Class Templates]]"
  - "[[Chapter 26 — Templates and Classes]]"
---

# Partial Template Specialization

**Partial template specialization** provides a customized class template implementation for a *subset* of type arguments — not a single fixed type, but a constrained family (e.g., all pointer types, or all `std::pair<T, T>` with both types equal).

## Key restriction: classes only

Partial specialization is **only available for class templates**, not for function templates:

```
template <typename T, typename U>
void foo(T, U) { }   // primary function template

template <typename T>
void foo(T, T) { }   // ✗ — partial specialization of a function template is NOT allowed
                      //     use overloads instead
```

For [[Function Template Specialization]], only full specialization (`template<>`) is permitted.

## Syntax

A partial specialization has a non-empty `template<>` header (some parameters remain generic) and a partially-specified class name:

```cpp
// Primary template: two independent type params
template <typename T, typename U>
class MyClass
{
    // general implementation
};

// Partial specialization: both types must be the same
template <typename T>
class MyClass<T, T>
{
    // implementation for the case T == U
};
```

The compiler selects the **most specialized** matching template. If `MyClass<int, int>` is requested, the partial specialization (`T == U`) wins over the primary.

## Common use case: pointer types

The most practical partial specialization matches any pointer type, allowing the class to store and manage raw pointers differently from non-pointer types. See [[Partial Template Specialization for Pointers]].

## Subnodes

- [[Partial Template Specialization for Pointers]] — partial specialization for `T*`

> Full coverage: [[Chapter 26 — Templates and Classes]] → Class Template Specialization → Partial Template Specialization
