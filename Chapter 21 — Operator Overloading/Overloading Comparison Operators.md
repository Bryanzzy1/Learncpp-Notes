---
tags:
  - cpp/operators
  - cpp/classes
  - concept
  - syntax
  - best-practice
aliases:
  - comparison operator overloading
  - operator== overload
  - operator< overload
  - spaceship operator
  - three-way comparison
  - operator<=>
up: "[[Chapter 21 — Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Operator Overload Selection Guide]]"
  - "[[Function Templates]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Overloading Comparison Operators

Comparison operators (`==`, `!=`, `<`, `>`, `<=`, `>=`) are typically overloaded as non-member functions (friend or normal) since they don't modify either operand.

**Design rule:** only define comparison operators that make intuitive sense for your class. Not every class has a meaningful ordering.

## Minimizing redundancy

Once `operator==` and `operator<` are defined, the others can be derived:

| Operator | Derived as |
|---|---|
| `!=` | `!(operator==)` |
| `>` | `operator<` with parameters swapped |
| `>=` | `!(operator<)` |
| `<=` | `!(operator>)` |

```cpp
bool operator!=(const MyClass& a, const MyClass& b) { return !(a == b); }
bool operator> (const MyClass& a, const MyClass& b) { return b < a; }
bool operator>=(const MyClass& a, const MyClass& b) { return !(a < b); }
bool operator<=(const MyClass& a, const MyClass& b) { return !(a > b); }
```

## The spaceship operator `<=>` (C++20)

`operator<=>` performs a **three-way comparison** and returns a value indicating the ordering:

| Return value | Meaning |
|---|---|
| `< 0` | Left operand is less than right |
| `== 0` | Operands are equal |
| `> 0` | Left operand is greater than right |

By defining `operator<=>` (and optionally `operator==`), the compiler can synthesize all six comparison operators automatically — reducing the work to at most two functions, sometimes just one.

## Interaction with function templates

Comparison operators are frequently required by standard library algorithms and [[Function Templates|function templates]] (e.g. `std::sort` uses `operator<`). Overloading `operator<` for your class makes it sortable and compatible with ordered containers.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Overloading Comparison Operators
