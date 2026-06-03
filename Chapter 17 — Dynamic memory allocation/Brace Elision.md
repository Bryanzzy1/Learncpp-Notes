---
tags:
  - cpp/arrays
  - cpp/aggregates
  - syntax
  - subnode
aliases:
  - brace elision
  - aggregate braces
  - omitting braces
up: "[[std-array]]"
related:
  - "[[std-array]]"
  - "[[Initialization]]"
  - "[[Structs]]"
---

# Brace Elision

**Brace elision** is a C++ rule that allows inner braces to be omitted when initializing aggregates (including `std::array`). Elision is optional — the compiler deduces which elements to initialize from the flat list of values.

## When elision is safe

You can omit inner braces when:

- Initializing `std::array` with **scalar (single) values**:
  ```cpp
  std::array<int, 3> a { 1, 2, 3 }; // inner braces not required
  ```

- Initializing with **class types or arrays where the element type is explicitly named per element** (CTAD or explicit type):
  ```cpp
  constexpr std::array houses {
      House{ 13, 1, 7 },
      House{ 14, 2, 5 },
  };
  ```

## When extra braces are required

When initializing `std::array<Struct, N>` **without** naming the type on each element, the compiler sees a flat list and cannot determine which values belong to which struct member. An extra pair of braces wraps the C-style array that `std::array` contains internally:

```cpp
constexpr std::array<House, 3> houses {{  // outer {} for std::array, inner {} for the C-array member
    { 13, 4, 30 },
    { 14, 3, 10 },
    { 15, 3, 40 },
}};
```

## Safe default

There is no harm in always using double braces — it avoids having to reason about when elision applies in each specific case.

See also [[Initialization]] for aggregate initialization rules, and [[Structs]] for struct member layout.

> Full coverage: [[Chapter 17 — Dynamic memory allocation]] → std::array
