---
tags:
  - cpp/arrays
  - cpp/enumerations
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - enumerator index
  - enum array index
  - count enumerator
up: "[[Arrays and Loops]]"
related:
  - "[[Arrays and Loops]]"
  - "[[Unscoped Enumerations]]"
  - "[[static_assert]]"
  - "[[Assertions]]"
  - "[[constexpr]]"
  - "[[std-vector]]"
---

# Enumerator Indexing

[[Unscoped Enumerations|Unscoped enumerators]] implicitly convert to `std::size_t`, making them usable as array indices. This technique documents the **semantic meaning** of each index rather than using raw integer literals.

```cpp
namespace Students
{
    enum Names
    {
        kenny,       // 0
        kyle,        // 1
        stan,        // 2
        butters,     // 3
        cartman,     // 4
        max_students // 5  <-- count enumerator
    };
}

std::vector testScores { 78, 94, 66, 77, 14 };
testScores[Students::stan] = 76; // meaning is self-documenting
```

## Count enumerator

Add a final enumerator (by convention named `max_<something>` or `count`) whose value equals the number of real enumerators. Its value is automatically one past the last real entry:

```cpp
enum Color { red, green, blue, max_colors }; // max_colors == 3
```

Use this count to size arrays and to assert that the array length matches:

```cpp
std::vector<int> palette(Colors::max_colors); // exactly the right number of slots
```

## Asserting on array length

Pair the count enumerator with an assertion to catch mismatches at compile time or runtime.

**For constexpr arrays** — use `static_assert` (see [[static_assert]]):

```cpp
constexpr std::array<std::string_view, Students::max_students> studentNames {
    "Kenny", "Kyle", "Stan", "Butters", "Cartman"
};
static_assert(std::size(studentNames) == Students::max_students,
              "studentNames length does not match Students enum");
```

**For non-constexpr arrays** — use a runtime `assert` (see [[Assertions]]):

```cpp
std::vector<int> scores { 78, 94, 66, 77, 14 };
assert(scores.size() == Students::max_students && "scores length mismatch");
```

## Non-constexpr enumerators

If the enumeration is defined inside a namespace or class and cannot be made constexpr, the same technique still works at runtime via `assert`. Only the compile-time `static_assert` requires a constexpr context (see [[constexpr]]).

> Full coverage: [[Chapter 16 — Arrays, Strings, Pointers]] → Enumerator Indexing
