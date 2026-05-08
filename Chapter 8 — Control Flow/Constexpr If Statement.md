---
tags:
  - cpp/control-flow
  - cpp/constexpr
  - concept
  - syntax
  - subnode
aliases:
  - constexpr if
  - if constexpr
  - compile-time if
up: "[[Conditional Statements]]"
related:
  - "[[Conditional Statements]]"
  - "[[constexpr]]"
  - "[[Compile-time Evaluation]]"
  - "[[constexpr Functions]]"
---

# Constexpr If Statement

Introduced in C++17, a **constexpr if statement** evaluates its condition at compile time rather than runtime.

```cpp
if constexpr (condition)
    true-statement;
else
    false-statement; // optional
```

- If the constexpr condition is `true`, the entire if-else is replaced by the **true-statement** during compilation.
- If the constexpr condition is `false`, the entire if-else is replaced by the **false-statement** (or removed entirely if there is no `else`).

The discarded branch is not compiled, which is useful for template code where one branch would otherwise fail to compile for certain types.

## Relationship to constexpr

The condition must be a compile-time constant expression — see [[constexpr]] and [[Compile-time Evaluation]]. This is the same requirement as [[constexpr Functions]] parameters.

## Contrast with a regular if

A normal `if` evaluates at runtime; both branches must be syntactically valid for all template instantiations. A `constexpr if` discards the dead branch entirely, so each branch only needs to be valid when selected.

> Full coverage: [[Chapter 8 — Control Flow]] → Constexpr If Statement
