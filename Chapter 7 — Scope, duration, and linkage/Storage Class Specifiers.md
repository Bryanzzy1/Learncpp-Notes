---
tags:
  - cpp/variables
  - cpp/linkage
  - concept
  - syntax
  - subnode
aliases:
  - storage class specifier
  - extern specifier
  - static specifier
  - thread_local
  - mutable
up: "[[Internal Linkage]]"
related:
  - "[[Internal Linkage]]"
  - "[[External Linkage]]"
  - "[[Static Local Variables]]"
  - "[[Scope, Duration, and Linkage Summary]]"
---

# Storage Class Specifiers

A **storage class specifier** sets both the linkage and storage duration of an identifier.

| **Specifier** | **Meaning** | **Note** |
| --- | --- | --- |
| `extern` | Static (or thread_local) storage duration and **external** linkage | |
| `static` | Static (or thread_local) storage duration and **internal** linkage | |
| `thread_local` | Thread storage duration | |
| `mutable` | Object allowed to be modified even if containing class is `const` | |
| `auto` | Automatic storage duration | Deprecated in C++11 |
| `register` | Automatic storage duration + hint to place in register | Deprecated in C++17 |

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Storage Class Specifiers
