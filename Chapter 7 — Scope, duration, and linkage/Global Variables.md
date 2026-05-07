---
tags:
  - cpp/variables
  - cpp/scope
  - concept
  - best-practice
aliases:
  - static duration
  - static variables
  - g_ prefix
  - initialization order problem
  - constant initialization
  - dynamic initialization
up: "[[Chapter 7 — Scope, duration, and linkage]]"
related:
  - "[[Local Variables]]"
  - "[[Internal Linkage]]"
  - "[[External Linkage]]"
  - "[[Static Local Variables]]"
  - "[[Inline Functions and Variables]]"
  - "[[Initialization]]"
  - "[[Memory Model]]"
  - "[[constexpr]]"
  - "[[Undefined Behavior]]"
---

# Global Variables

Global variables are declared outside any function and live for the entire program lifetime.

## Storage duration

Global variables have **static duration**: created before `main()` begins, destroyed when the program ends. They are sometimes called **static variables**. They live in static storage separate from the stack — see [[Memory Model]].

## Best practices

- Prefer defining global variables **inside a namespace** rather than in the global namespace.
- Use a `g_` (or `g`) prefix to distinguish globals from locals and parameters.
- **(Non-const) global variables are generally considered harmful** — they make program state hard to reason about and can cause subtle [[Undefined Behavior]] through the initialization order problem.

## Initialization order problem

Global variable initialization happens in two phases. See [[Initialization]] for the general initialization model.

### Phase 1 — Static initialization

1. **Constant initialization**: globals with [[constexpr]] initializers are initialized to those values.
2. **Zero-initialization**: globals without initializers are zero-initialized (also considered static initialization since `0` is a constexpr value).

### Phase 2 — Dynamic initialization

Globals with non-`constexpr` initializers are initialized here. The order across translation units is **unspecified**.

**Rules:**
- Avoid initializing globals with static duration using other globals from a different translation unit.
- Avoid dynamic initialization of globals whenever possible.

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Global Variables
