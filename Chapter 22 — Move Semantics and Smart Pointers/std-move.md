---
tags:
  - cpp/memory
  - cpp/types
  - concept
  - syntax
  - best-practice
aliases:
  - std::move
  - move cast
  - std::move_if_noexcept
up: "[[Chapter 22 — Move Semantics and Smart Pointers]]"
related:
  - "[[Move Semantics]]"
  - "[[Rvalue References]]"
  - "[[Move Constructor and Assignment]]"
  - "[[static_cast]]"
  - "[[Value Categories]]"
  - "[[Chapter 22 — Move Semantics and Smart Pointers]]"
---

# std::move

`std::move` is a standard library function (in `<utility>`) that casts its argument to an **rvalue reference**, enabling [[Move Semantics]] to be invoked on a named object. It does not itself move anything — it just changes the value category of the expression so that a move constructor or move assignment can be selected.

## How it works

`std::move(x)` is equivalent to `static_cast<T&&>(x)` — it uses [[static_cast]] to cast the argument to an rvalue reference. This is the standard way to tell the compiler "I'm done with this object; you may transfer its resources."

## Typical usage

```cpp
T tmp { std::move(a) }; // invokes move constructor — a loses ownership
a = std::move(b);       // invokes move assignment — b loses ownership
b = std::move(tmp);     // invokes move assignment — tmp loses ownership
```

This three-way swap pattern transfers resources without copying.

## Usage rules

- Only call `std::move()` on a persistent object whose value you intend to transfer.
- **Do not read from or rely on the value of the moved-from object** after the call — it is in a valid but unspecified state.
- `std::move` on a temporary (already an rvalue) is redundant and can suppress copy elision — avoid it on return values.

## std::move_if_noexcept

`std::move_if_noexcept(x)` is a conditional variant:
- Returns an **rvalue reference** (enabling a move) if the type has a `noexcept` move constructor.
- Returns an **lvalue reference** (falling back to copy) otherwise.

This is used by the standard library to provide the strong exception guarantee during operations like `std::vector` reallocation.

> Full coverage: [[Chapter 22 — Move Semantics and Smart Pointers]] → std::move
