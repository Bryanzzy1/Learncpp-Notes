---
tags:
  - cpp/arrays
  - cpp/enumerations
  - syntax
  - best-practice
aliases:
  - enum array indexing
  - count enumerator
  - enumeration array lookup
up: "[[Chapter 17 — Dynamic memory allocation]]"
related:
  - "[[std-array]]"
  - "[[Unscoped Enumerations]]"
  - "[[Enumerator Indexing]]"
  - "[[constexpr]]"
---

# Enumerator Array Indexing

Using [[Unscoped Enumerations|unscoped enumerators]] as `std::array` indices — building on the pattern introduced in [[Enumerator Indexing]] — enables self-documenting array access and compile-time safety checks.

## Static assert on initializer count

A **count enumerator** placed at the end of an enum tracks how many enumerators precede it. Use `static_assert` to guarantee the array length matches:

```cpp
namespace Pet
{
    enum Names { cat, dog, bird, max_pets };
}

constexpr std::array<std::string_view, Pet::max_pets> petNames { "cat", "dog", "bird" };
static_assert(petNames.size() == Pet::max_pets, "petNames length mismatch");
```

If a new enumerator is added to `Names` without a corresponding entry in `petNames`, the `static_assert` fires at compile time.

## Constexpr array for enumeration name lookup

A `constexpr std::array` of names, indexed by enumerator value, enables two-way conversion:

1. **Enumerator → name**: `petNames[Pet::dog]` returns `"dog"`.
2. **Name → enumerator**: iterate the array and match the name to recover the enumerator index.

```cpp
for (std::size_t i { 0 }; i < petNames.size(); ++i)
    std::cout << i << ": " << petNames[i] << '\n';
```

## Range-based for with enumerators

To iterate over all enumerators with a range-based for loop, store the enumerators themselves in a `constexpr std::array`:

```cpp
constexpr std::array pets { Pet::cat, Pet::dog, Pet::bird };

for (auto pet : pets)
    std::cout << petNames[pet] << '\n';
```

> Full coverage: [[Chapter 17 — Dynamic memory allocation]] → std::array and enumerations
