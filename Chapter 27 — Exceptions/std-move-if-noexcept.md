---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - cpp/memory
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - std::move_if_noexcept
  - move_if_noexcept
up: "[[noexcept]]"
related:
  - "[[noexcept]]"
  - "[[Move Semantics]]"
  - "[[Move Constructor and Assignment]]"
  - "[[std-move]]"
  - "[[Smart Pointers]]"
  - "[[Chapter 27 — Exceptions]]"
---

# std::move_if_noexcept

`std::move_if_noexcept` (in `<utility>`) is a conditional move utility that prefers moving over copying, but only when the move is guaranteed not to throw — preserving the **strong exception guarantee**.

## The problem: throwing moves break the strong guarantee

A move transfers ownership from source to destination. If an exception fires mid-move:
- The destination is in an indeterminate state.
- The source has already been modified (possibly partially emptied).
- Rolling back is impossible — the strong guarantee is violated.

Copies don't have this problem because the source is untouched until the copy completes. But copies are slower.

## How `std::move_if_noexcept` works

```cpp
auto result = std::move_if_noexcept(obj);
```

| Move constructor of `obj`'s type | Return type | Effect |
|---|---|---|
| `noexcept` | rvalue reference (`T&&`) | Move proceeds |
| Potentially throwing | lvalue reference (`const T&`) | Copy is used instead |

If a copy constructor is also unavailable, `move_if_noexcept` falls back to a move regardless (there is no alternative), waiving the strong guarantee.

## Standard library use

`std::vector::resize()` and similar operations internally use `std::move_if_noexcept` when reallocating their buffer. If your type has a `noexcept` move constructor, `std::vector` will move elements during reallocation (fast path). If not, it copies them (safe but slower).

This is why the [[noexcept]] best practice says: **always mark move constructors `noexcept`** — it unlocks the fast path for standard containers.

## Relationship to noexcept

`std::move_if_noexcept` is the runtime-visible consequence of the `noexcept` specifier. Together, they let you write code that is:
- Fast when moves are safe (`noexcept`)
- Correct when moves might throw (fallback to copy)

See [[Move Constructor and Assignment]] for how to write and mark move operations, and [[std-move|std::move]] for the unconditional move cast.

> Full coverage: [[Chapter 27 — Exceptions]] → noexcept → std::move_if_noexcept
