---
tags:
  - cpp/constexpr
  - cpp/functions
  - cpp/headers
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - constexpr implicitly inline
  - consteval implicitly inline
  - constexpr header rule
up: "[[Constexpr Function Rules]]"
related:
  - "[[Constexpr Function Rules]]"
  - "[[Inline Functions and Variables]]"
  - "[[Header Files]]"
  - "[[One Definition Rule]]"
  - "[[Translation Unit]]"
  - "[[constexpr]]"
  - "[[Consteval]]"
---

# Constexpr Functions Implicitly Inline

All `constexpr` (and `consteval`) functions are **implicitly inline**.

## What this means

Because compile-time evaluation requires the compiler to see the **full function definition** — not just a forward declaration — constexpr functions must be visible in every translation unit that calls them. The `inline` designation is what allows identical definitions to exist in multiple [[Translation Unit|translation units]] without violating the [[One Definition Rule]].

## Placement rules

| Usage | Where to define |
|---|---|
| Single `.cpp` file only | Define in the `.cpp` file, **above** the first call site |
| Multiple `.cpp` files | Define in a **[[Header Files|header file]]**; `#include` it everywhere needed |

The precise requirement: *"the constexpr function must be defined prior to the outermost evaluation that eventually results in the invocation."* In practice this means the definition must appear before any call that triggers compile-time evaluation.

## Contrast with regular `inline`

See [[Inline Functions and Variables]] for the modern meaning of `inline` (multiple identical definitions allowed). Constexpr functions get this property automatically — no explicit `inline` keyword needed.

> Full coverage: [[Chapter F — Constexpr functions]] → Constexpr Function Rules
