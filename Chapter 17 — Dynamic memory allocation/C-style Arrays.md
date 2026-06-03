---
tags:
  - cpp/arrays
  - cpp/c-style
  - concept
  - syntax
  - best-practice
aliases:
  - C-style array
  - raw array
  - built-in array
  - fixed array
  - C array
up: "[[Chapter 17 — Dynamic memory allocation]]"
related:
  - "[[Array Decay]]"
  - "[[Pointer Arithmetic]]"
  - "[[std-array]]"
  - "[[Pointers]]"
  - "[[Initialization]]"
  - "[[Memory Model]]"
  - "[[Undefined Behavior]]"
---

# C-style Arrays

A **C-style array** is a fixed-size sequence container built into C++ (no `#include` required). It stores elements contiguously in [[Memory Model|memory]], just like `std::array`, but lacks many of its safety features.

```cpp
int testScore[30] {};   // 30 value-initialized ints
```

The length must be a **constant expression** for stack-allocated (automatic storage) arrays. Heap-allocated C-style arrays (`new int[n]`) may have a runtime length, and a length of 0 is legal.

## Initialization

C-style arrays are aggregates and support [[Initialization|aggregate initialization]]:

```cpp
int primes[] { 2, 3, 5, 7, 11 }; // length deduced as 5
```

Prefer omitting the explicit length whenever you initialize every element, so the compiler keeps the count accurate.

## Size queries

| Expression | Result | Available since |
|---|---|---|
| `sizeof(arr)` | Total bytes used by the array | Always |
| `std::size(arr)` | Length as `std::size_t` | C++17 |
| `std::ssize(arr)` | Length as signed (`std::ptrdiff_t`) | C++20 |

`sizeof` does **not** decay the array, so it returns the total byte count, not the pointer size.

## No assignment

C-style arrays do not support copy-assignment:

```cpp
int a[] { 1, 2, 3 };
int b[] { 4, 5, 6 };
a = b;  // error
```

## Indexing

C-style arrays accept signed or unsigned indices, including unscoped enumerations (implicit conversion to `std::size_t`). There is no bounds checking — an out-of-bounds access is [[Undefined Behavior]].

## Multidimensional arrays

```cpp
int grid[3][4] {};        // 3 rows, 4 columns
int cube[4][4][4] {};     // 4×4×4
```

C++ stores multidimensional arrays in **row-major order**: elements within a row are adjacent in [[Memory Model|memory]], rows are laid out sequentially.

```
[0][0] [0][1] [0][2] [0][3]
[1][0] [1][1] [1][2] [1][3]
[2][0] [2][1] [2][2] [2][3]
```

Some languages (e.g., Fortran) use column-major order instead.

## Array decay

When used in most expressions, a C-style array implicitly converts to a pointer to its first element — see [[Array Decay]]. This is the primary source of C-style array pitfalls.

## Recommendation

Avoid C-style arrays in new code. Prefer [[std-array|std::array]] for fixed-size arrays and `std::vector` for dynamic arrays. The only common legitimate use is string literals and interoperating with C APIs.

> Full coverage: [[Chapter 17 — Dynamic memory allocation]] → C-style Arrays
