---
tags:
  - cpp/preprocessing
  - concept
  - syntax
  - subnode
aliases:
  - "#ifdef"
  - "#ifndef"
  - "#if"
  - "#endif"
  - "#if 0"
  - conditional compilation
up: "[[Preprocessing]]"
related:
  - "[[Macros]]"
  - "[[Header Files]]"
  - "[[Debugging]]"
  - "[[Preprocessing]]"
---

# Conditional Compilation

Preprocessor directives that include or exclude blocks of code before compilation, based on whether a macro is defined.

```cpp
#define PRINT_JOE

#ifdef PRINT_JOE
    std::cout << "Joe\n";   // compiled — PRINT_JOE is defined
#endif

#ifdef PRINT_BOB
    std::cout << "Bob\n";   // excluded — PRINT_BOB is not defined
#endif
```

## Key Directives

| Directive | Meaning |
|-----------|---------|
| `#ifdef X` | Include block if `X` is defined — same as `#if defined(X)` |
| `#ifndef X` | Include block if `X` is **not** defined — same as `#if !defined(X)` |
| `#endif` | Closes a conditional block |
| `#if 0` / `#endif` | Permanently excludes a block (comment-out alternative) |

## Scope Rules

- `#define` directives are **not block-scoped** — a macro defined anywhere is active from that point to the end of the file
- Directives are only valid from their point of definition to the end of the file they are defined in

[[Header Files|Header guards]] (`#ifndef HEADER_H / #define HEADER_H / #endif`) are the most common practical use of conditional compilation.

> Full coverage: [[Chapter 2 — Functions and Files]] → Preprocessing
