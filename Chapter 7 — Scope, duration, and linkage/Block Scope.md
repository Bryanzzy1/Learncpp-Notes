---
tags:
  - cpp/scope
  - concept
aliases:
  - nested blocks
  - block nesting
  - scope nesting
up: "[[Chapter 7 — Scope, duration, and linkage]]"
related:
  - "[[Local Variables]]"
  - "[[Variable Shadowing]]"
  - "[[Undefined Behavior]]"
---

# Block Scope

A **block** (compound statement) defines a local scope. Code inside `{}` can see variables declared in the same block or any enclosing block.

## Nesting limit

The C++ standard requires compilers to support at least **256 levels of nested blocks**. In practice this limit is rarely approached.

## Key rules

- Variables declared in a block are destroyed when that block exits.
- Inner blocks can access variables from outer blocks, but not vice versa.
- Declaring the same identifier in an inner block **shadows** the outer one — see [[Variable Shadowing]].

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Block Scope
