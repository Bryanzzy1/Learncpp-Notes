---
tags:
  - cpp/error-handling
  - cpp/debugging
  - concept
  - best-practice
aliases:
  - defensive programming
up: "[[Chapter 9 — Error handling]]"
related:
  - "[[Assertions]]"
  - "[[Handling Errors in Functions]]"
  - "[[Informal Testing]]"
  - "[[Undefined Behavior]]"
  - "[[Functions]]"
---

# Defensive Programming

**Defensive programming** is the practice of anticipating all the ways software can be misused — by end-users, by calling code, or by the programmer themselves — and writing code that handles those cases explicitly.

The guiding principle: assume that callers will pass bad input, that invariants may be violated, and that edge cases will occur. Write [[Functions|functions]] that enforce their own preconditions rather than relying on callers to "just know."

## Techniques

- **[[Assertions]]** — document and enforce preconditions at runtime (`assert()`) or compile time (`static_assert`). Catch programmer errors early.
- **Input validation** — check user-supplied or external data at system boundaries before it enters your logic.
- **Error propagation** — when a function detects invalid state, communicate that failure to the caller rather than silently continuing (see [[Handling Errors in Functions]]).

## Scope

Defensive programming is distinct from test coverage (see [[Informal Testing]]): testing verifies that code works under expected conditions, while defensive programming makes the code resilient against unexpected conditions — including scenarios that trigger [[Undefined Behavior]] in naive implementations.

> Full coverage: [[Chapter 9 — Error handling]] → Defensive Programming
