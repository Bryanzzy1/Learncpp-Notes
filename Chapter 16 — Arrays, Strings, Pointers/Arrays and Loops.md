---
tags:
  - cpp/arrays
  - cpp/iteration
  - concept
  - syntax
  - best-practice
aliases:
  - array indexing
  - array iteration
  - array loops
up: "[[Chapter 16 — Arrays, Strings, Pointers]]"
related:
  - "[[std-vector]]"
  - "[[Range-based For Loops]]"
  - "[[Enumerator Indexing]]"
  - "[[For Loop]]"
  - "[[Loops]]"
  - "[[size_t]]"
  - "[[Undefined Behavior]]"
---

# Arrays and Loops

## The signed/unsigned index mismatch

`std::vector::size()` returns an **unsigned** [[size_t|`std::size_t`]] value. Using a signed loop counter creates a sign-conversion warning and potential [[Undefined Behavior]] at large sizes.

Three approaches to handle this cleanly:

### 1. Cast size to a signed type

```cpp
for (int i = 0; i < static_cast<int>(arr.size()); ++i)
    std::cout << arr[i];
```

### 2. Use std::ptrdiff_t (C++17)

`std::ptrdiff_t` is the standard signed counterpart to `std::size_t`. Define a type alias to avoid the awkward name:

```cpp
using Index = std::ptrdiff_t;

for (Index i{ 0 }; i < static_cast<Index>(arr.size()); ++i)
    std::cout << arr[i];
```

### 3. Use std::ssize() (C++20)

`std::ssize()` returns the container length as a signed type (typically `std::ptrdiff_t`), making the cast unnecessary:

```cpp
for (auto i{ 0 }; i < std::ssize(arr); ++i)
    std::cout << arr[i];
```

## Best practice

Avoid explicit array indexing wherever possible — prefer [[Range-based For Loops]] for read/write traversal, and [[Enumerator Indexing]] when an index has semantic meaning.

## Dependent names

Inside a template, any name that depends on a template parameter is a **dependent name**. When a dependent name refers to a type, prefix it with `typename`:

```cpp
template <typename T>
void printSize(const T& container)
{
    typename T::size_type len = container.size(); // T::size_type is a dependent name
}
```

## Subnodes

- [[Range-based For Loops]] — for-each syntax and element reference patterns
- [[Enumerator Indexing]] — using enumerations as semantically meaningful indices

> Full coverage: [[Chapter 16 — Arrays, Strings, Pointers]] → Arrays and Loops
