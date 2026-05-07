---
tags:
  - cpp/variables
  - cpp/scope
  - concept
aliases:
  - automatic variables
  - automatic storage duration
  - storage duration
  - linkage
up: "[[Chapter 7 — Scope, duration, and linkage]]"
related:
  - "[[Global Variables]]"
  - "[[Static Local Variables]]"
  - "[[Internal Linkage]]"
  - "[[External Linkage]]"
---

# Local Variables

Local variables are variables declared inside a function or block.

## Storage duration

A variable's **storage duration** (or just **duration**) determines when it is created and destroyed:

- Local variables have **automatic storage duration**: created when the block is entered, destroyed when it exits.
- For this reason, local variables are sometimes called **automatic variables**.

## Linkage

An identifier's **linkage** determines whether the same name in a different scope refers to the same object:

- Local variables have **no linkage** — each declaration is independent, even if two locals share a name.

## Contrast with global variables

| Property | Local variable | Global variable |
|----------|---------------|-----------------|
| Duration | Automatic | Static |
| Linkage  | None | Internal or External |
| Lifetime | Block | Program |

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Local Variables
