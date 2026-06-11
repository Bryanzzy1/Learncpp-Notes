---
tags:
  - cpp/operators
  - cpp/templates
  - concept
  - syntax
  - subnode
aliases:
  - operator overloading with templates
  - template operator requirements
up: "[[Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Function Templates]]"
  - "[[Overloading Comparison Operators]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Operators and Function Templates

[[Function Templates|Function templates]] frequently use operators on their template type parameters. If you instantiate such a template with a user-defined type, that type must have the required operators overloaded — otherwise the instantiation fails to compile.

```cpp
template <typename T>
const T& max(const T& x, const T& y)
{
    return (x < y) ? y : x; // requires operator< on T
}
```

Passing a user-defined type `MyClass` to `max` will compile only if `MyClass` has `operator<` defined.

## Implication for class design

If you intend to use your class with standard library algorithms (e.g. `std::sort`, `std::min`, `std::max`) or any generic code that uses comparison or arithmetic operators, overload those operators on your class. The most commonly required are:

- `operator<` — for ordering algorithms and ordered containers
- `operator==` — for equality checks and unordered containers
- `operator+`, `operator-`, etc. — for numeric/accumulation algorithms

See [[Overloading Comparison Operators]] for how to define a consistent set of comparison operators efficiently.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Operator Overloading → Operators and Function Templates
