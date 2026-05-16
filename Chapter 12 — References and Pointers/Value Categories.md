---
tags:
  - cpp/expressions
  - cpp/types
  - concept
aliases:
  - lvalue
  - rvalue
  - value category
  - lvalue-to-rvalue conversion
  - modifiable lvalue
  - non-modifiable lvalue
up: "[[Chapter 12 — References and Pointers]]"
related:
  - "[[Compound Data Types]]"
  - "[[Lvalue References]]"
  - "[[Pointers]]"
  - "[[Undefined Behavior]]"
  - "[[Memory Model]]"
  - "[[Standard Conversions]]"
---

# Value Categories

Every C++ expression has a **value category** that determines how it can be used in context.

## Lvalues

An **lvalue** ("locator value") is an expression that evaluates to an **identifiable object or function** — one that can be differentiated from other entities, typically by its address. Lvalues generally have a lifetime longer than a single expression.

Lvalues come in two forms:
- **Modifiable lvalue** — the value can be changed (non-const variable)
- **Non-modifiable lvalue** — the value cannot be changed (const or constexpr variable)

Lvalues can be accessed via an identifier, reference, or pointer. See [[Memory Model]] for how storage relates to identity.

## Rvalues

An **rvalue** ("right value") is an expression that is **not** an lvalue — it evaluates to a temporary value with no persistent identity. Examples: integer literals, the return value of a function that returns by value.

Rvalues are not identifiable: they exist only within the expression in which they appear and cannot be addressed.

Unless otherwise specified, operators expect rvalue operands.

## Lvalue-to-rvalue conversion

When an **rvalue is expected but an lvalue is provided**, the compiler performs an implicit lvalue-to-rvalue conversion: the lvalue is evaluated to produce its value (an rvalue). This is one of the [[Standard Conversions]] (value transformations category).

## C-style string literals

A C-style string literal (e.g. `"hello"`) is an **lvalue** because C-style strings are arrays, and arrays decay to a pointer — a pointer is an identifiable object.

> Full coverage: [[Chapter 12 — References and Pointers]] → Value Categories
