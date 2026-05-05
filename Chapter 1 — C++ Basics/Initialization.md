---
tags:
  - cpp/initialization
  - concept
  - syntax
aliases:
  - brace initialization
  - list initialization
  - copy initialization
  - direct initialization
  - value initialization
  - zero initialization
  - instantiation
up: "[[Chapter 1 — C++ Basics]]"
related:
  - "[[static_cast]]"
  - "[[Memory Model]]"
  - "[[Pass by Value]]"
  - "[[constexpr]]"
  - "[[List Initialization]]"
  - "[[Undefined Behavior]]"
---

# Initialization

Giving an object a known value at the point of definition.

| Form | Syntax | Notes |
|------|--------|-------|
| Default | `int a;` | No initializer; value is indeterminate for non-class types |
| Copy | `int b = 5;` | Fallen out of favor; may be inefficient for complex types |
| Direct | `int c(6);` | Also used when explicitly casting via [[static_cast]] |
| **[[List Initialization\|List (brace)]]** | `int d { 7 };` | **Preferred** — disallows narrowing conversions at compile time |
| Value / zero | `int e {};` | Zero-initializes (`0` for integers, `false` for `bool`) |

- **Instantiation**: a variable has been created (allocated) and initialized; the object occupies [[Memory Model|memory]] for its lifetime
- An uninitialized variable holds an indeterminate value → [[Undefined Behavior|undefined behavior]]
- `const` variables *must* be initialized at the point of definition
- `constexpr` variables require an initializer that is itself a compile-time constant — see [[constexpr]]

> Full coverage: [[Chapter 1 — C++ Basics]] → Initialization
