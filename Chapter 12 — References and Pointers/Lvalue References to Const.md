---
tags:
  - cpp/references
  - cpp/const
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - const lvalue reference
  - reference to const
  - lvalue reference to const
  - const reference
up: "[[Lvalue References]]"
related:
  - "[[Lvalue References]]"
  - "[[Value Categories]]"
  - "[[Pass by Reference]]"
  - "[[constexpr]]"
  - "[[Static Local Variables]]"
  - "[[Global Variables]]"
  - "[[Initialization]]"
  - "[[Chapter 12 — References and Pointers]]"
---

# Lvalue References to Const

An **lvalue reference to const** (commonly called a **const reference**) is declared with the `const` keyword and treats its referent as immutable, regardless of whether the underlying object is actually const:

```cpp
const int& ref { x }; // ref sees x as const — cannot modify x through ref
```

**Prefer lvalue references to const over lvalue references to non-const** unless you explicitly need to modify the referent.

## Broader binding ability

Unlike non-const lvalue references (which can only bind to modifiable lvalues), a const lvalue reference can bind to:
- Modifiable lvalues
- Non-modifiable (const) lvalues
- **Rvalues** — e.g. `const int& ref { 5 };`

When bound to an rvalue or a value requiring implicit conversion, the compiler creates a **temporary object** and the reference binds to that temporary:

```cpp
const double& r1 { 5 };  // temporary double with value 5; r1 binds to it
const int& r2 { 'a' };   // temporary int initialized from 'a'; r2 binds to it
```

Any subsequent modifications to the original object are **not** visible through the reference (it points to the temporary, not the original), and vice-versa.

## Temporary lifetime extension

When a const lvalue reference is **directly** bound to a temporary object, the temporary's lifetime is extended to match the lifetime of the reference.

## constexpr references

Applying `constexpr` to a reference allows it in constant expressions (see [[constexpr]]), but imposes a restriction: the reference can **only bind to objects with static storage duration** — [[Global Variables|globals]] or [[Static Local Variables|static locals]]. Local variables have addresses unknown at compile time, so a `constexpr` reference cannot bind to them. These see limited practical use.

> Full coverage: [[Chapter 12 — References and Pointers]] → Lvalue References
