---
tags:
  - cpp/constexpr
  - cpp/functions
  - concept
  - syntax
  - best-practice
aliases:
  - constexpr function
  - compile-time function rules
up: "[[Chapter F — Constexpr functions]]"
related:
  - "[[Constexpr Function Parameters]]"
  - "[[Constexpr Functions Implicitly Inline]]"
  - "[[Constexpr Function Capabilities]]"
  - "[[Consteval]]"
  - "[[Pure Functions]]"
  - "[[constexpr]]"
  - "[[constexpr Functions]]"
  - "[[Compile-time Evaluation]]"
  - "[[Functions]]"
---

# Constexpr Function Rules

A **constexpr function** is a function allowed to be called in a constant expression. It can evaluate at **compile time** or **runtime** depending on context.

## Compile-time evaluation conditions

For a constexpr function to evaluate at compile time, **both** conditions must hold:

1. All arguments to the call must be constant expressions (known at compile time)
2. Every statement and expression within the function body must be evaluatable at compile time

When either condition fails, the function evaluates at runtime exactly like a normal function — the `constexpr` keyword has no effect in that case.

> Compile-time evaluation is only **guaranteed** when a constant expression is required (e.g. initializing a [[constexpr]] variable or a `static_assert`). In other contexts the compiler may or may not evaluate it at compile time.

## Testing constexpr functions

Always verify a constexpr function in a context that **requires** a constant expression:

```cpp
constexpr int square(int x) { return x * x; }

constexpr int s { square(4) }; // forces compile-time evaluation — will catch any compile-time failure
```

A function may work fine when evaluated at runtime but fail when the compiler attempts compile-time evaluation. Relying only on runtime tests is insufficient.

## Relationship to Ch5 constexpr

Chapter F builds on the [[constexpr Functions]] note from Ch5. The key additions here are:
- The precise two-condition rule for compile-time evaluation
- Behavior when conditions aren't met (silent runtime fallback)
- The guarantee distinction: required contexts vs. opportunistic contexts

See [[Compile-time Evaluation]] for how the compiler optimizes constant expressions in general.

> Full coverage: [[Chapter F — Constexpr functions]] → Constexpr Function Rules
