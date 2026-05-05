---
tags:
  - cpp/preprocessing
  - concept
  - syntax
aliases:
  - preprocessor
  - preprocessor directives
  - macros
  - "#define"
  - header guards
up: "[[Chapter 2 — Functions and Files|Chapter 2 — Functions and Files]]"
related:
  - "[[constexpr]]"
  - "[[Functions]]"
  - "[[Macros]]"
  - "[[Conditional Compilation]]"
  - "[[Header Files]]"
  - "[[Translation Unit]]"
---

# Preprocessing

The preprocessor runs before the compiler, performing text substitution. The result is called a **[[Translation Unit]]**.

**Core directives:**

| Directive | Purpose |
|-----------|---------|
| `#include "file.h"` | Replace with contents of the file (searches current dir first) |
| `#include <header>` | Replace with a standard/system header |
| `#define ID text` | [[Macros\|Object-like macro]]: replaces every `ID` with `text` |
| `#ifdef` / `#ifndef` / `#endif` | [[Conditional Compilation]] |
| `#if 0` ... `#endif` | Exclude a block from compilation |
| `#pragma once` | Prevent a header from being included more than once |

**Header guards** (alternative to `#pragma once`):
```cpp
#ifndef MY_HEADER_H
#define MY_HEADER_H
// declarations
#endif
```

**Macros vs constants:**
Prefer `const`/[[constexpr]] variables over `#define` for named constants — macros have no type, no scope, and cannot be inspected by the debugger.

> Full coverage: [[Chapter 2 — Functions and Files|Chapter 2 — Functions and Files]] → Preprocessing and Header Files
