---
tags:
  - cpp/functions
  - cpp/variables
  - concept
  - syntax
  - best-practice
aliases:
  - inline expansion
  - inline keyword
  - overhead
  - ODR
  - one definition rule
up: "[[Chapter 7 — Scope, duration, and linkage]]"
related:
  - "[[Global Variables]]"
  - "[[External Linkage]]"
---

# Inline Functions and Variables

## Overhead

**Overhead** is the extra work required to set up, facilitate, and clean up around a task. For function calls this includes stack frame setup, argument passing, and return handling.

## Inline expansion

**Inline expansion** replaces a function call with the body of the called function at the call site, eliminating call overhead.

**Cost:** if the function body is larger than the call instruction, each expansion grows the executable. Larger executables tend to be slower (worse cache utilization).

## The `inline` keyword — modern meaning

Do **not** use `inline` to request inline expansion — compilers ignore this hint and decide on their own. Premature optimization via `inline` can hurt performance.

In modern C++, `inline` means **"multiple definitions are allowed"**:
- An inline function/variable can be defined in multiple translation units without violating the **One Definition Rule (ODR)**.
- All definitions must be **identical** (or undefined behavior results).
- Inline functions are typically defined in **header files** so every translation unit that uses them can see the full definition.

## Inline constexpr global variables (C++17)

```cpp
// constants.h
inline constexpr int maxItems { 100 };
```

Prefer `inline constexpr` global variables in header files when you need shared compile-time constants across translation units.

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Inline Functions and Variables
