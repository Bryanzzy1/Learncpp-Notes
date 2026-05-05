---
tags:
  - cpp/functions
  - cpp/scope
  - concept
  - subnode
aliases:
  - ODR
  - one definition rule
  - declaration vs definition
  - forward declaration
up: "[[Functions]]"
related:
  - "[[Header Files]]"
  - "[[Preprocessing]]"
  - "[[Namespaces]]"
---

# One Definition Rule (ODR)

Every identifier in C++ may be **declared** any number of times but **defined** exactly once across the entire program.

| Term | Meaning | Example |
|------|---------|---------|
| Declaration | Tells the compiler an identifier exists and its type | `void foo();` |
| Definition | Implements a function or creates a variable | `void foo() { }` / `int x;` |
| Pure declaration | A declaration that is not a definition | `void foo();` (forward declaration) |
| Initialization | Provides an initial value for a defined object | `int x { 2 };` |

- Every definition is also a declaration.
- A **forward declaration** (`void foo();`) satisfies the compiler without providing a definition — the linker finds the definition later, which must exist in exactly one translation unit.
- Violating the ODR (defining the same entity in two translation units) is a **linker error**.

> Full coverage: [[Chapter 2 — Functions and Files]] → Forward Declaration
