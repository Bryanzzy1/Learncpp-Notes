---
tags:
  - cpp/control-flow
  - concept
  - syntax
  - best-practice
aliases:
  - if statement
  - else statement
  - if-else
  - dangling else
  - null statement
up: "[[Chapter 8 — Control Flow]]"
related:
  - "[[Constexpr If Statement]]"
  - "[[Switch Statements]]"
  - "[[Goto Statements]]"
  - "[[Control Flow]]"
  - "[[Undefined Behavior]]"
  - "[[Macros]]"
---

# Conditional Statements

`if`/`else` statements execute a sequence of code only when a condition is met. They are one category of [[Control Flow]].

## Nesting and blocks

Always enclose nested `if` bodies in a block `{}` to avoid ambiguity about which `if` an `else` belongs to.

## Null statement

A **null statement** is an expression statement consisting of just a semicolon — it performs no action:

```cpp
if (x > 10)
    ; // null statement — does nothing
```

To mimic Python's `pass` keyword with the preprocessor (via [[Macros]]):

```cpp
#define PASS

void foo(int x, int y)
{
    if (x > y)
        PASS;
    else
        PASS;
}
```

## Dangling else

A **dangling else** occurs when it is ambiguous which `if` an `else` is paired with. C++ resolves the ambiguity by matching `else` to the last unmatched `if` in the same block. Prevent it by always putting `if` bodies in explicit blocks.

## Subnodes

- [[Constexpr If Statement]] — compile-time conditional branch (C++17)
- [[Switch Statements]] — multi-way branching on integral or enum values
- [[Goto Statements]] — unconditional jumps via statement labels

> Full coverage: [[Chapter 8 — Control Flow]] → Conditional Statements
