---
tags:
  - cpp/algorithms
  - cpp/iterators
  - cpp/containers
  - concept
  - syntax
  - best-practice
aliases:
  - standard library algorithms
  - std::sort
  - std::find
  - std::find_if
  - std::count
  - std::count_if
  - std::for_each
  - algorithm library
  - <algorithm>
up: "[[Chapter 18 — Iterators and Algorithms]]"
related:
  - "[[Iterators]]"
  - "[[Function Templates]]"
  - "[[std-array|std::array]]"
  - "[[std-vector|std::vector]]"
  - "[[Range-based For Loops]]"
---

# Standard Library Algorithms

The `<algorithm>` header provides a collection of functions that operate on **iterator ranges** — a begin iterator and a one-past-the-end iterator — rather than on containers directly. This makes them container-agnostic: the same algorithm works on `std::array`, `std::vector`, C-style arrays, and any other range with compatible [[Iterators]].

**Prefer algorithm functions over hand-written loops** — they are better tested, often better optimized, and clearly communicate intent.

## std::sort

Sorts elements in ascending order (by default):

```cpp
int array[]{ 30, 50, 20, 10, 40 };
std::sort(std::begin(array), std::end(array));  // ascending
```

### Custom comparator

Pass a binary predicate that returns `true` when the first argument should come before the second:

```cpp
bool greater(int a, int b)
{
    return (a > b);   // order a before b if a > b → descending
}

std::array arr{ 13, 90, 99, 5, 40, 80 };
std::sort(arr.begin(), arr.end(), greater);
```

The standard library ships `std::greater<>{}` (from `<functional>`) as a ready-made descending comparator.

## std::find

Searches for the **first occurrence** of a value. Returns an iterator to the found element, or `end` if not found:

```cpp
auto found{ std::find(arr.begin(), arr.end(), targetValue) };
if (found != arr.end())
    std::cout << *found << '\n';
```

## std::find_if

Like `std::find`, but tests a **predicate** (a callable returning `bool`) instead of equality:

```cpp
bool containsNut(std::string_view str)
{
    return str.find("nut") != std::string_view::npos;
}

std::array<std::string_view, 5> arr{ "apple", "banana", "walnut", "lemon", "peanut" };
auto found{ std::find_if(arr.begin(), arr.end(), containsNut) };
```

Any callable — function pointer, function object, or lambda — works as a predicate. See [[Function Templates]] for how algorithms are defined as templates.

## std::count and std::count_if

Count elements equal to a value, or satisfying a predicate:

```cpp
auto nuts{ std::count_if(arr.begin(), arr.end(), containsNut) };
```

`std::count` takes a value; `std::count_if` takes a predicate.

## std::for_each

Applies a function to every element in the range:

```cpp
void doubleNumber(int& i) { i *= 2; }

std::array arr{ 1, 2, 3, 4 };
std::for_each(arr.begin(), arr.end(), doubleNumber);
```

The callable receives each element by reference if the parameter is a reference; mutations are applied to the container. Compare with [[Range-based For Loops]], which is often cleaner for simple iterations.

## Performance and execution order

Before choosing an algorithm, check its documentation for:
- **Complexity guarantee** (e.g. `std::sort` is O(n log n))
- **Execution order** — some algorithms do not guarantee element-processing order

## C++20 Ranges

C++20 introduces the `std::ranges` namespace, which allows passing the container directly instead of a begin/end pair:

```cpp
std::ranges::sort(arr);          // C++20: no begin/end needed
std::ranges::find(arr, value);
```

This is shorter and eliminates mismatched iterator bugs.

> Full coverage: [[Chapter 18 — Iterators and Algorithms]] → standard library algorithms
