---
tags:
  - cpp/scope
  - cpp/linkage
  - cpp/variables
  - concept
aliases:
  - variable summary table
  - scope summary
  - linkage summary
  - duration summary
up: "[[Chapter 7 — Scope, duration, and linkage]]"
related:
  - "[[Local Variables]]"
  - "[[Global Variables]]"
  - "[[Internal Linkage]]"
  - "[[External Linkage]]"
  - "[[Static Local Variables]]"
  - "[[Forward Declaration Summary]]"
  - "[[Storage Class Specifiers]]"
---

# Scope, Duration, and Linkage Summary

| **Type** | **Example** | **Scope** | **Duration** | **Linkage** | **Notes** |
| --- | --- | --- | --- | --- | --- |
| Local variable | `int x;` | Block | Automatic | None | |
| Static local variable | `static int s_x;` | Block | Static | None | |
| Dynamic local variable | `int* x { new int{} };` | Block | Dynamic | None | |
| Function parameter | `void foo(int x)` | Block | Automatic | None | |
| Internal non-const global | `static int g_x;` | Global | Static | Internal | Initialized or uninitialized |
| External non-const global | `int g_x;` | Global | Static | External | Initialized or uninitialized |
| Inline non-const global (C++17) | `inline int g_x;` | Global | Static | External | Initialized or uninitialized |
| Internal constant global | `constexpr int g_x { 1 };` | Global | Static | Internal | Must be initialized |
| External constant global | `extern const int g_x { 1 };` | Global | Static | External | Must be initialized |
| Inline constant global (C++17) | `inline constexpr int g_x { 1 };` | Global | Static | External | Must be initialized |

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Scope, Duration, and Linkage Summary
