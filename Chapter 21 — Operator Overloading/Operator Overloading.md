---
tags:
  - cpp/operators
  - cpp/classes
  - concept
  - syntax
  - best-practice
aliases:
  - overloaded operator
  - operator overloading rules
  - overloading rules
up: "[[Chapter 21 — Operator Overloading]]"
related:
  - "[[Friend Function Overloads]]"
  - "[[Normal Function Overloads]]"
  - "[[Member Function Overloads]]"
  - "[[Operator Overload Selection Guide]]"
  - "[[Operators and Function Templates]]"
  - "[[Function Overloading]]"
  - "[[Overload Resolution]]"
  - "[[Implicit Type Conversion]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Operator Overloading

**Operator overloading** lets you define custom behavior for a built-in operator when applied to a user-defined type. The overload is just a function named `operator<symbol>` that the compiler selects via [[Overload Resolution|overload resolution]].

> This note covers Chapter 21's deep dive into overloading rules. For the introductory treatment (including `operator<<` for enums/structs), see the [[Chapter 13 — Compound Types|Chapter 13]] note on [[Chapter 13 — Compound Types/Operator Overloading|Operator Overloading]].

## How the compiler dispatches operators

- If **all operands are fundamental types**: the compiler calls a built-in routine (if one exists); otherwise a compile error.
- If **any operand is a program-defined type** (class or enum): the compiler uses [[Function Overloading|function overload resolution]] to find the best match among overloaded `operator` functions. This step may also involve implicit conversion of program-defined types to fundamental types via an [[Overloading Typecasts|overloaded typecast]].

## Hard rules

| Rule | Detail |
|---|---|
| Only existing operators | You cannot invent new operator symbols. |
| At least one user-defined operand | `operator+(int, MyString)` is legal; `operator+(int, double)` is not. |
| Operand count is fixed | You cannot change whether an operator is unary or binary. |

## Return value conventions

| Operator kind | Return |
|---|---|
| Does **not** modify its operands (e.g. `+`, `==`) | By value |
| Modifies its **leftmost** operand (e.g. `+=`, `++`, `=`) | By reference to leftmost operand |

## Design guideline

Keep the semantic meaning of the operator consistent with its built-in behavior. An `operator+` that subtracts is confusing and error-prone.

## Implementation methods

There are three ways to implement an overloaded operator — see the subnodes for details:

- [[Friend Function Overloads]] — non-member with private access
- [[Normal Function Overloads]] — non-member, uses public interface
- [[Member Function Overloads]] — left operand is implicit `*this`

Use the [[Operator Overload Selection Guide]] to decide which to use.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Operator Overloading
