---
tags:
  - cpp/linkage
  - cpp/variables
  - concept
  - subnode
aliases:
  - forward declaration table
  - forward declaration reference
up: "[[External Linkage]]"
related:
  - "[[External Linkage]]"
  - "[[Internal Linkage]]"
  - "[[Scope, Duration, and Linkage Summary]]"
  - "[[Functions]]"
  - "[[Header Files]]"
  - "[[One Definition Rule]]"
---

# Forward Declaration Summary

Forward declarations let the compiler know an identifier exists before its definition is seen — the linker resolves the actual definition later. The [[One Definition Rule]] governs how many definitions are allowed. [[Header Files]] are the standard place to put forward declarations so they can be shared across translation units.

| **Type** | **Example** | **Notes** |
| --- | --- | --- |
| Function forward declaration | `void foo(int x);` | Prototype only, no function body |
| Non-constant variable forward declaration | `extern int g_x;` | Must be uninitialized |
| Const variable forward declaration | `extern const int g_x;` | Must be uninitialized |
| Constexpr variable forward declaration | `extern constexpr int g_x;` | Not allowed — `constexpr` cannot be forward declared |

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Forward Declaration Summary
