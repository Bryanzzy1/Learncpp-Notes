---
tags:
  - cpp/arrays
  - cpp/containers
  - concept
  - syntax
  - best-practice
aliases:
  - std::array
  - fixed-size array
  - array class
up: "[[Chapter 17 — Dynamic memory allocation]]"
related:
  - "[[Brace Elision]]"
  - "[[Enumerator Array Indexing]]"
  - "[[C-style Arrays]]"
  - "[[std-vector|std::vector]]"
  - "[[constexpr]]"
  - "[[CTAD]]"
  - "[[Function Templates]]"
  - "[[Non-type Template Parameters]]"
  - "[[Alias Templates]]"
  - "[[Structs]]"
  - "[[Initialization]]"
  - "[[size_t]]"
---

# std::array

`std::array` is a fixed-size, stack-allocated container that wraps a C-style array. Its size is part of its type and must be a **constant expression**. Use `std::array` for constexpr arrays; use [[std-vector|std::vector]] for arrays whose size isn't known at compile time.

```cpp
std::array<int, 5> a {};   // 5 value-initialized ints
```

A zero-length `std::array` is legal — it is a special-case class with no data.

## Initialization

`std::array` is an aggregate and uses [[Initialization|aggregate initialization]]:

- No initializer → elements are **default initialized** (usually uninitialized for fundamental types).
- More initializers than length → **compiler error**.
- Fewer initializers than length → remaining elements are **value initialized** (zero for fundamental types).

### CTAD

Use [[CTAD|class template argument deduction]] to let the compiler deduce both element type and length:

```cpp
constexpr std::array a1 { 9, 7, 5, 3, 1 }; // std::array<int, 5>
constexpr std::array a2 { 9.7, 7.31 };     // std::array<double, 2>
```

### std::to_array

`std::to_array` lets you specify the type but omit the length. It creates a temporary `std::array` then copy-initializes the target — this is more expensive than a direct declaration. Avoid it inside loops.

```cpp
constexpr auto arr1 { std::to_array<int, 5>({ 9, 7, 5, 3, 1 }) }; // type + size
constexpr auto arr2 { std::to_array<int>({ 9, 7, 5, 3, 1 }) };    // type only
constexpr auto arr3 { std::to_array({ 9, 7, 5, 3, 1 }) };         // full deduction
```

## Length and indexing

`size_type` for `std::array` is always `std::size_t` (see [[size_t]]). The suffix `0UZ` produces a `std::size_t` literal.

| Access method | Bounds checking | Notes |
|---|---|---|
| `operator[]` | None | Fast, undefined behavior on out-of-bounds |
| `at()` | Runtime (throws `std::out_of_range`) | Slower but safe |
| `std::get<N>()` | Compile-time | Only usable with constexpr index |

```cpp
std::cout << std::get<3>(prime); // ok
std::cout << std::get<9>(prime); // compile error — invalid index
```

## Passing std::array

Because `std::array` encodes its length in its type, passing arrays of different lengths requires a [[Function Templates|function template]]. Use a [[Non-type Template Parameters|non-type template parameter]] for the length:

```cpp
template <typename T, std::size_t N>
void process(const std::array<T, N>& arr)
{
    static_assert(N != 0);
    std::cout << arr[0] << '\n';
}
```

Using `auto` for the non-type parameter (C++17) deduces the type of `N`:

```cpp
template <typename T, auto N>
void process(const std::array<T, N>& arr)
{
    static_assert(N != 0);
    std::cout << arr[0] << '\n';
}
```

## Returning std::array

Three options, in order of preference:

| Strategy | When to use |
|---|---|
| Return by value | Array is small, element type is cheap to copy, non-performance-critical code |
| Out parameter (pass by ref) | Caller already owns the array |
| Return `std::vector` instead | Large arrays or performance-sensitive context — [[std-vector\|std::vector]] is move-capable |

## std::array of structs

When initializing a `std::array` of [[Structs|structs]] without naming the type on each element, you need an extra pair of braces (see [[Brace Elision]]):

```cpp
constexpr std::array<House, 3> houses {{
    { 13, 4, 30 },
    { 14, 3, 10 },
    { 15, 3, 40 },
}};
```

CTAD avoids this requirement:

```cpp
constexpr std::array houses {
    House{ 13, 1, 7 },
    House{ 14, 2, 5 },
    House{ 15, 2, 4 },
};
```

## Two-dimensional arrays

Nest `std::array` instances and simplify with an [[Alias Templates|alias template]]:

```cpp
template <typename T, std::size_t Row, std::size_t Col>
using Array2d = std::array<std::array<T, Col>, Row>;

Array2d<int, 3, 4> grid {};  // 3 rows × 4 cols
```

### std::mdspan (C++23)

`std::mdspan` provides a **multidimensional view** over a contiguous sequence — it does not own the data. Unlike `std::string_view`, it allows modification of the underlying elements if they are non-const.

```cpp
std::mdspan mdView { arr.data(), 3, 4 };
std::size_t rows { mdView.extents().extent(0) };
std::size_t cols { mdView.extents().extent(1) };

// C++23 multi-index operator[]
std::cout << mdView[row, col] << '\n';
```

C++26 will add `std::mdarray`, which combines ownership (`std::array`) and multidimensional access (`std::mdspan`).

> Full coverage: [[Chapter 17 — Dynamic memory allocation]] → std::array
