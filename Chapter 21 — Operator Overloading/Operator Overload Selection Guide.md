---
tags:
  - cpp/operators
  - cpp/classes
  - best-practice
  - subnode
aliases:
  - operator overload decision
  - when to use friend or member overload
  - overload method selection
up: "[[Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Friend Function Overloads]]"
  - "[[Normal Function Overloads]]"
  - "[[Member Function Overloads]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Operator Overload Selection Guide

Use this decision table to choose between member, normal, and friend implementations.

| Situation | Use |
|---|---|
| Overloading `=`, `[]`, `()`, or `->` | **Member function** (language requirement) |
| Overloading any **unary** operator | **Member function** |
| Binary operator that does **not** modify its left operand (e.g. `+`, `==`, `<<`) | **Normal function** (preferred) or friend |
| Binary operator that modifies its left operand, but you **cannot** modify the left operand's class (e.g. `<<` with `std::ostream`) | **Normal function** (preferred) or friend |
| Binary operator that modifies its left operand, and you **can** modify the left operand's class (e.g. `+=`) | **Member function** |

## Priority ordering

1. **Normal function** — fewest privileges; preferred whenever the public interface is sufficient.
2. **Friend function** — when private access is needed but member is not required.
3. **Member function** — when the language mandates it, or for operators modifying `*this`.

For more detail on each approach, see [[Friend Function Overloads]], [[Normal Function Overloads]], and [[Member Function Overloads]].

> Full coverage: [[Chapter 21 — Operator Overloading]] → Operator Overloading → Operator Overload Selection Guide
