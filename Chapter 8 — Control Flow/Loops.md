---
tags:
  - cpp/control-flow
  - cpp/loops
  - concept
  - syntax
  - best-practice
aliases:
  - while loop
  - loop
  - loop variable
  - iteration
up: "[[Chapter 8 — Control Flow]]"
related:
  - "[[Do-While Loop]]"
  - "[[For Loop]]"
  - "[[Control Flow]]"
  - "[[Goto Statements]]"
  - "[[Undefined Behavior]]"
---

# Loops

Loops repeatedly execute a sequence of statements zero or more times until a condition is no longer met. They are the structured alternative to jumping with [[Goto Statements]].

C++ provides three loop constructs:

| Loop | Executes at least once? | Best for |
|------|------------------------|----------|
| `while` | No | Unknown iteration count |
| `do-while` | **Yes** | Must execute body first |
| `for` | No | Known iteration count / index |

See [[Do-While Loop]] and [[For Loop]] for the variants with distinct syntax.

## Loop variables

Integral loop variables should generally be a **signed** integral type. Using an unsigned type can cause subtle wrap-around bugs — for example, a loop that decrements to zero and then checks `>= 0` will never end with an unsigned variable, which is [[Undefined Behavior]]-adjacent behavior.

## Subnodes

- [[Do-While Loop]] — loop that always executes its body at least once
- [[For Loop]] — compact loop with init, condition, and increment in one line

> Full coverage: [[Chapter 8 — Control Flow]] → Loops
