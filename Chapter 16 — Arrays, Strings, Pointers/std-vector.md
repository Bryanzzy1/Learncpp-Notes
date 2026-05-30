---
tags:
  - cpp/arrays
  - cpp/containers
  - cpp/vectors
  - concept
  - syntax
  - best-practice
aliases:
  - std::vector
  - vector
  - dynamic array
  - std::vector<T>
up: "[[Chapter 16 — Arrays, Strings, Pointers]]"
related:
  - "[[Containers and Arrays]]"
  - "[[Move Semantics]]"
  - "[[Stack Operations]]"
  - "[[std-vector-bool]]"
  - "[[Initialization]]"
  - "[[List Initialization]]"
  - "[[size_t]]"
  - "[[static_cast]]"
  - "[[Narrowing Conversions]]"
  - "[[CTAD]]"
  - "[[Function Templates]]"
  - "[[constexpr]]"
  - "[[Undefined Behavior]]"
---

# std::vector

`std::vector<T>` is C++'s primary **dynamic array** — a heap-allocated, resizable container. Its length can grow and shrink at runtime.

## Construction

```cpp
std::vector<int> empty{};                              // value-init: 0 elements
std::vector<int> primes{ 2, 3, 5, 7 };                // list construction: 4 elements
std::vector vowels { 'a', 'e', 'i', 'o', 'u' };       // CTAD (C++17): deduces char
std::vector<int> data( 10 );                           // direct-init: 10 zero-initialized ints
```

[[List Initialization]] with `{}` triggers the **list constructor** — it treats the braces as an initializer list, setting length = number of values. [[CTAD]] (C++17) lets the compiler deduce the element type from the initializer values.

`std::vector<T>(std::size_t n)` is `explicit`, so it must be invoked with direct-init `( )` — copy-init `= 10` won't compile.

### Init disambiguation

When `{}` is used with a single value, the list constructor wins over the explicit length constructor:

```cpp
std::vector<int> v{ 10 };    // list constructor: one element with value 10
std::vector<int> v( 10 );    // explicit length constructor: 10 zero-initialized elements
```

When constructing with non-element values (e.g., a length), always use direct initialization `( )`.

For class members, use:

```cpp
struct Foo {
    std::vector<int> v{ std::vector<int>(8) }; // 8-element vector as member default
};
```

## const vectors

A `const std::vector` must be initialized and cannot be modified afterward — its elements are treated as `const`. However, the element type itself must not be declared `const` (`std::vector<const int>` is ill-formed).

`std::vector` cannot be made `constexpr`. Use `std::array` when you need a [[constexpr]] array.

## Length and size_type

`std::vector::size_type` is almost always an alias for [[size_t|`std::size_t`]] — a large **unsigned** integral type. This creates a mismatch when indexing with signed integers.

| Method | Return type | Notes |
|--------|------------|-------|
| `.size()` | `size_type` (unsigned) | Standard member function |
| `std::size(v)` (C++17) | `size_type` (unsigned) | Also works on C-style arrays |
| `std::ssize(v)` (C++20) | `std::ptrdiff_t` (signed) | Preferred for signed-index loops |

## Accessing elements

```cpp
v[2];          // operator[]: no bounds check — UB if out-of-range (see [[Undefined Behavior]])
v.at(2);       // at(): throws std::out_of_range if out-of-range (slower but safe)
v.data()[idx]; // data(): returns raw pointer, bypasses sign-conversion warning
```

When indexing with a **constexpr signed int**, the compiler can implicitly convert to `size_type` without a [[Narrowing Conversions|narrowing conversion]] warning. For runtime signed indices, prefer `data()[idx]` or [[static_cast]]`<std::size_t>(idx)`.

## Passing by reference

Always pass `std::vector` by (const) reference to avoid expensive copies. Use a [[Function Templates|function template]] to accept vectors of any element type:

```cpp
template <typename T>
void process(const std::vector<T>& arr)
{
    std::cout << arr[0] << '\n';
}
// C++20 abbreviated form:
void process(const auto& arr) { ... }
```

`std::vector` length cannot be `static_assert`'d (it's not constexpr). Prefer `std::array` when you need compile-time length guarantees, or avoid writing functions that depend on a minimum length.

## Capacity vs length

- **length**: number of elements currently in use (`.size()`)
- **capacity**: number of elements for which storage is allocated (`.capacity()`)

```cpp
v.resize(n);         // changes length (and capacity if needed)
v.reserve(n);        // changes capacity only — does NOT change length
v.shrink_to_fit();   // requests capacity == length (non-binding)
```

When length reaches capacity and a new element is inserted, the vector **reallocates** — allocates a larger block and moves all elements. This is O(n) but amortized O(1) due to geometric growth.

Vector indexing is based on **length**, not capacity — elements beyond the current length are inaccessible even if capacity is larger.

## Subnodes

- [[Move Semantics]] — copy vs move when returning std::vector by value
- [[Stack Operations]] — push_back, pop_back, emplace_back as a stack interface
- [[std-vector-bool]] — the specialization to avoid

> Full coverage: [[Chapter 16 — Arrays, Strings, Pointers]] → std::vector
