---
tags:
  - cpp/linkage
  - cpp/variables
  - concept
aliases:
  - forward declaration table
  - forward declaration reference
up: "[[Chapter 7 — Scope, duration, and linkage]]"
related:
  - "[[External Linkage]]"
  - "[[Internal Linkage]]"
  - "[[Scope, Duration, and Linkage Summary]]"
---

# Forward Declaration Summary

| **Type** | **Example** | **Notes** |
| --- | --- | --- |
| Function forward declaration | `void foo(int x);` | Prototype only, no function body |
| Non-constant variable forward declaration | `extern int g_x;` | Must be uninitialized |
| Const variable forward declaration | `extern const int g_x;` | Must be uninitialized |
| Constexpr variable forward declaration | `extern constexpr int g_x;` | Not allowed — `constexpr` cannot be forward declared |

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Forward Declaration Summary
