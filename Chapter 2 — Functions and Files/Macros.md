---
tags:
  - cpp/preprocessing
  - cpp/macros
  - concept
  - syntax
  - subnode
aliases:
  - macro
  - "#define"
  - object-like macro
  - function-like macro
  - substitution text
up: "[[Preprocessing]]"
related:
  - "[[Preprocessing]]"
  - "[[Constants]]"
  - "[[Conditional Compilation]]"
  - "[[Header Files]]"
---

# Macros

A **macro** is a preprocessor rule defined with `#define` that replaces matching identifiers with text before the compiler sees the code.

## Object-like Macros

```cpp
#define IDENTIFIER
#define IDENTIFIER substitution_text
```

- `#define MY_NAME "Alex"` — every occurrence of `MY_NAME` in the file is replaced with `"Alex"`
- Names should be written in `ALL_CAPS_WITH_UNDERSCORES`
- **Prefer `const`/`constexpr` variables** — macros have no type, no scope, and cannot be inspected by the debugger

## Function-like Macros

Act like functions but expand inline. Generally unsafe and error-prone — prefer real inline functions.

## Key Behaviors

- The preprocessor removes all `#define` lines before compilation — the compiler never sees them, only the substituted output
- Macros defined with no substitution text (e.g. `#define PRINT_JOE`) are used for [[Conditional Compilation]]
- Directive scope: a `#define` is active from its point of definition to the end of the file regardless of block scope

> Full coverage: [[Chapter 2 — Functions and Files]] → Preprocessing
