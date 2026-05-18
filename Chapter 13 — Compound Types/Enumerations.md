---
tags:
  - cpp/types
  - cpp/enums
  - concept
  - syntax
aliases:
  - enumeration
  - enum
  - enumerated type
  - enumerator
up: "[[Chapter 13 — Compound Types]]"
related:
  - "[[Unscoped Enumerations]]"
  - "[[Scoped Enumerations]]"
  - "[[Operator Overloading]]"
  - "[[Program-defined Types]]"
  - "[[static_cast]]"
  - "[[Namespaces]]"
---

# Enumerations

An **enumeration** (also called an **enumerated type** or **enum**) is a compound type whose values are restricted to a set of named symbolic constants called **enumerators**. Every enumerated type is a **distinct type** — unlike [[Type Aliases|type aliases]], the compiler treats it as separate from all other types.

C++ has two flavors:

| | [[Unscoped Enumerations]] (`enum`) | [[Scoped Enumerations]] (`enum class`) |
|---|---|---|
| Enumerator scope | Leaks into enclosing namespace | Contained in enum's scope |
| Implicit int conversion | Yes | No |
| Preferred? | Only when necessary | Prefer |

## Common rules

- Enumerators default to integer values starting at 0; each subsequent enumerator is one more than the previous.
- Avoid assigning explicit values unless you have a compelling reason.
- To convert between an enum and its underlying integer, use [[static_cast]].
- The specific integral type used for enumerator values is the **underlying type** (or **base**); specify it only when necessary (e.g. `enum Color : std::int8_t`).
- To avoid naming collisions, prefer placing enumerations inside a [[Namespaces|named scope region]] (namespace or class).

> Full coverage: [[Chapter 13 — Compound Types]] → Enumerations
