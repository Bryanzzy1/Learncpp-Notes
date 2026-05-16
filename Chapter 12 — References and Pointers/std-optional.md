---
tags:
  - cpp/utilities
  - cpp/types
  - concept
  - syntax
  - best-practice
aliases:
  - std::optional
  - optional
  - std::nullopt
  - std::expected
  - has_value
  - value_or
up: "[[Chapter 12 — References and Pointers]]"
related:
  - "[[Pointers]]"
  - "[[Pass by Address]]"
  - "[[In and Out Parameters]]"
  - "[[Type Deduction with References and Pointers]]"
  - "[[Undefined Behavior]]"
---

# std::optional

`std::optional<T>` (C++17, header `<optional>`) represents a value of type `T` that may or may not be present — a type-safe alternative to using a pointer or sentinel value to signal "no value."

## Comparison with pointers

| Behavior | Pointer | `std::optional` |
| --- | --- | --- |
| Hold no value | `{}` or `nullptr` | `{}` or `std::nullopt` |
| Hold a value | assign an address | assign a value directly |
| Check if has value | implicit bool conversion | bool conversion or `.has_value()` |
| Get value | dereference (`*`) | dereference (`*`) or `.value()` |

## Accessing the value

```cpp
std::optional<int> o1 { 5 };
std::optional<int> o2 {};      // no value (nullopt)
std::optional<int> o3 { 42 };

std::cout << *o1;              // undefined behavior if empty
std::cout << o2.value();       // throws std::bad_optional_access if empty
std::cout << o3.value_or(0);   // returns 0 if o3 is empty
```

Prefer `.value_or()` when a fallback is natural. Use `.value()` when an empty optional is a programming error. Avoid bare dereference (`*`) without a prior check — it is [[Undefined Behavior]] when empty.

## When to use

- Return `std::optional<T>` from functions that may fail to produce a value, instead of a sentinel (e.g. `-1`, `nullptr`).
- Prefer **function overloading** for optional function parameters when possible (cleaner call sites).
- Otherwise, use `std::optional<T>` for optional arguments when `T` is normally passed by value.
- Favor `const T*` ([[Pass by Address]]) when `T` is expensive to copy — wrapping it in `optional` copies the whole object.

## std::expected (C++23)

`std::expected<T, E>` extends the concept: a function returns either an expected value (`T`) or an unexpected error (`E`), providing richer failure information than `std::optional` which can only express "present or absent."

> Full coverage: [[Chapter 12 — References and Pointers]] → std::optional
