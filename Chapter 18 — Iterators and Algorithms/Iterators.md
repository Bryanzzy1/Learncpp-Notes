---
tags:
  - cpp/iterators
  - cpp/containers
  - concept
  - syntax
aliases:
  - iterator
  - iterator invalidation
  - std::begin
  - std::end
  - begin
  - end
up: "[[Chapter 18 — Iterators and Algorithms]]"
related:
  - "[[Standard Library Algorithms]]"
  - "[[Pointers]]"
  - "[[Pointer Arithmetic]]"
  - "[[Array Decay]]"
  - "[[std-array|std::array]]"
  - "[[std-vector|std::vector]]"
  - "[[Range-based For Loops]]"
  - "[[Undefined Behavior]]"
---

# Iterators

An **iterator** is an object designed to traverse through a container, providing access to each element along the way. Iterators are the bridge between containers and [[Standard Library Algorithms|algorithms]] — algorithms operate on iterator ranges, not containers directly.

## Pointers as iterators

The simplest iterator is a raw pointer. For any array, a pointer to the first element serves as the begin iterator, and a pointer one-past-the-end serves as the end sentinel:

```cpp
std::array arr{ 0, 1, 2, 3, 4, 5, 6 };

auto begin{ &arr[0] };
auto end{ begin + std::size(arr) };  // one past last element

for (auto ptr{ begin }; ptr != end; ++ptr)
{
    std::cout << *ptr << ' ';
}
```

This works because [[Pointer Arithmetic]] scales increments by the element size, so `++ptr` advances to the next element. This is also exactly how [[Array Decay|decayed]] C-style array pointers behave.

## std::begin and std::end

The standard library provides `std::begin` and `std::end` as a uniform interface:

| Container type | Header providing `begin`/`end` |
|---|---|
| C-style arrays | `<iterator>` |
| `std::array`, `std::vector`, and other containers | The container's own header (e.g. `<array>`, `<vector>`) |

```cpp
int array[]{ 30, 50, 20, 10, 40 };
std::sort(std::begin(array), std::end(array));  // works on raw arrays
```

Member `.begin()` / `.end()` are equivalent for containers that provide them.

## Iterator convention: operator!=

By convention, iterator loops use `operator!=` (not `<`) to test termination:

```cpp
for (auto it{ arr.begin() }; it != arr.end(); ++it)
```

This works for all iterator categories, including those that don't support `<` (e.g. linked-list iterators).

## Iterator invalidation

An iterator becomes **invalidated** if the elements it refers to change address or are destroyed. Using an invalidated iterator is [[Undefined Behavior|undefined behavior]].

Common invalidation causes with [[std-vector|std::vector]]:
- Inserting or removing elements can trigger reallocation, invalidating all iterators.
- `erase()` invalidates iterators at and after the erased position.

`erase()` returns an iterator to the element one past the erased element, which allows safe continued iteration:

```cpp
auto it{ v.begin() };
while (it != v.end())
{
    if (shouldRemove(*it))
        it = v.erase(it);   // returns valid next iterator
    else
        ++it;
}
```

[[Range-based For Loops]] use iterators internally — modifying the container inside such a loop risks invalidation.

> Full coverage: [[Chapter 18 — Iterators and Algorithms]] → Iterators
