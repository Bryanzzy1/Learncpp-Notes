---
tags:
  - cpp/arrays
  - cpp/iteration
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - range-based for
  - for-each loop
  - for-each
  - ranged for
up: "[[Arrays and Loops]]"
related:
  - "[[Arrays and Loops]]"
  - "[[For Loop]]"
  - "[[Type Deduction]]"
  - "[[Lvalue References]]"
  - "[[std-vector]]"
  - "[[Undefined Behavior]]"
---

# Range-based For Loops

A **range-based for loop** (also called a for-each loop) iterates over every element in a container without managing an index or iterator manually.

```cpp
for (element_declaration : array_object)
    statement;
```

Example:

```cpp
std::vector fibonacci { 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89 };

for (auto num : fibonacci)       // num is a copy of each element
    std::cout << num << ' ';
```

## Element type guidelines

Use [[Type Deduction]] (`auto`) and choose the reference category based on intent:

| Declaration | When to use |
|-------------|-------------|
| `auto` | Modify copies of elements (original unchanged) |
| `auto&` | Modify the original elements in-place |
| `const auto&` | Read-only access — preferred default |

```cpp
for (const auto& word : words)   // read each string without copying
    std::cout << word << '\n';

for (auto& score : scores)       // increment each score in-place
    ++score;
```

Using `const auto&` avoids unnecessary copies of large types and is the correct default for read-only traversal. See [[Lvalue References]] for reference semantics.

## What range-based for works with

Range-based for works with any type that provides `begin()` and `end()` (or non-member equivalents):

- `std::vector`, `std::array`, `std::string`
- Non-decayed C-style arrays (the compiler knows the length)
- Linked lists, trees, maps

**Does not work** with **decayed** C-style arrays (passed as a pointer) — the compiler loses the length information, so `begin`/`end` cannot be deduced.

## Reverse iteration (C++20)

Use `std::views::reverse` to iterate in reverse order without index arithmetic:

```cpp
#include <ranges>

for (const auto& word : std::views::reverse(words))
    std::cout << word << '\n';
```

## Dependent names in templates

When writing a template that uses a container's nested type (e.g., `size_type`), prefix it with `typename`:

```cpp
template <typename T>
void print(const T& container)
{
    typename T::value_type elem{};   // dependent name — must use typename
}
```

Any name that depends on a template parameter is a **dependent name** and requires `typename` when used as a type.

> Full coverage: [[Chapter 16 — Arrays, Strings, Pointers]] → Range-based For Loops
