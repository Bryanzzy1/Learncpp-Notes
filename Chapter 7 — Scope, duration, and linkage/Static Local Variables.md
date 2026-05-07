---
tags:
  - cpp/variables
  - cpp/scope
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - static local variable
  - s_ prefix
  - static duration local
up: "[[Local Variables]]"
related:
  - "[[Local Variables]]"
  - "[[Global Variables]]"
  - "[[Storage Class Specifiers]]"
  - "[[Initialization]]"
  - "[[Memory Model]]"
---

# Static Local Variables

Using the `static` keyword on a local variable changes its duration from **automatic** to **static**. The variable is created at program start and destroyed at program end — but its scope remains limited to the block where it is declared. These variables live in static storage rather than the stack — see [[Memory Model]].

```cpp
void incrementAndPrint()
{
    static int s_count { 0 };  // initialized only once
    ++s_count;
    std::cout << s_count << '\n';
}
```

Each call retains the previous value of `s_count` — it persists across calls.

## Initialization

Static local variables are **initialized only the first time** the code executes (not on subsequent calls). Always provide an initializer — see [[Initialization]] for why uninitialized variables are dangerous.

## Naming convention

Prefix static local variables with `s_` to distinguish them from regular locals.

## When to use

- Avoid expensive local object initialization on every call by initializing once.
- Static locals can be `const` or `constexpr`, which is safe and common for lookup tables or one-time computed constants.

## When to avoid

Non-const static local variables should generally be avoided. If you do use them, ensure the variable never needs to be reset and is not used to alter program flow.

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Static Local Variables
