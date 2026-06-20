---
tags:
  - cpp/expressions
  - cpp/types
  - concept
  - syntax
aliases:
  - rvalue reference
  - "&&"
  - r-value reference
  - rvalue ref
  - rvalue reference variable
up: "[[Chapter 22 — Move Semantics and Smart Pointers]]"
related:
  - "[[Value Categories]]"
  - "[[Lvalue References]]"
  - "[[Move Semantics]]"
  - "[[std-move|std::move]]"
  - "[[Move Constructor and Assignment]]"
  - "[[Chapter 22 — Move Semantics and Smart Pointers]]"
---

# Rvalue References

An **rvalue reference** (declared with `&&`) is a reference that can only bind to an **rvalue** — a temporary or value with no persistent identity. Introduced in C++11, rvalue references are the mechanism that makes [[Move Semantics]] possible.

## Syntax

```cpp
int x{ 5 };
int& lref{ x };   // lvalue reference — binds to lvalue x
int&& rref{ 5 };  // rvalue reference — binds to rvalue 5
```

Unlike [[Lvalue References|lvalue references to const]] (which can also extend a temporary's lifetime), non-const rvalue references allow **modifying** the rvalue they bind to.

## Lifetime extension

An rvalue reference extends the lifetime of the temporary it is initialized with to the lifetime of the rvalue reference itself. This is analogous to how an lvalue reference to const can extend a temporary's lifetime.

## When initializing from a literal

When an rvalue reference is bound to a literal value, a temporary object is constructed from that literal. The reference then refers to that temporary, not to the literal itself.

## Rvalue reference variables are lvalues

A variable of rvalue reference type is itself an **lvalue** — it has a name and a persistent identity. This is the key reason [[std-move|std::move]] is needed: to explicitly cast a named rvalue-reference variable back to an rvalue so move semantics can be invoked again.

```cpp
void process(int&& r) {
    // r is an lvalue here, even though its type is int&&
    // std::move(r) is needed to pass r as an rvalue to another function
}
```

## Don't return rvalue references

Almost never return an rvalue reference from a function — for the same reason you almost never return an lvalue reference: the referenced object may be destroyed before the caller can use it.

> Full coverage: [[Chapter 22 — Move Semantics and Smart Pointers]] → Rvalue References
