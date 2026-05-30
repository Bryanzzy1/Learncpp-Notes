---
tags:
  - cpp/vectors
  - cpp/containers
  - concept
  - subnode
aliases:
  - move semantics
  - copy semantics
  - move ownership
  - move-capable type
up: "[[std-vector]]"
related:
  - "[[std-vector]]"
  - "[[Copy Constructor]]"
  - "[[Copy Elision]]"
  - "[[Return by Value]]"
  - "[[Pass by Value]]"
---

# Move Semantics

## Copy semantics

**Copy semantics** are the rules that determine how copies of objects are made. When a `std::vector` is copied, a full copy of all its heap-allocated elements is performed — this is O(n) and expensive.

## Move semantics

**Move semantics** are the rules that determine how data is transferred (not duplicated) from one object to another. When data is moved, **ownership transfers** — the source object is left in a valid but empty state.

The cost of a move is typically trivial: just two or three pointer assignments, regardless of how many elements the container holds.

## When move semantics are invoked

Move semantics apply when all three conditions hold:

1. The type supports move semantics (e.g., `std::vector`, `std::string`).
2. The object is initialized from (or assigned) an **rvalue** (temporary).
3. The move is not elided (see [[Copy Elision]]).

```cpp
std::vector<int> make() { return std::vector<int>{ 1, 2, 3 }; }

std::vector<int> v = make(); // move semantics (or copy elision) — no full copy
```

## Returning std::vector by value

Because `std::vector` supports move semantics, returning it by value is efficient:

```cpp
std::vector<int> generate(int n)
{
    std::vector<int> result;
    result.reserve(n);
    for (int i = 0; i < n; ++i)
        result.push_back(i);
    return result; // move (or NRVO copy elision) — not a full copy
}
```

The compiler either elides the copy entirely ([[Copy Elision|NRVO]]) or invokes move semantics to transfer ownership. Either way, no O(n) element copy occurs. Contrast with [[Return by Value]] for non-move-capable types.

> Full coverage: [[Chapter 16 — Arrays, Strings, Pointers]] → Move Semantics
