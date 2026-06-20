---
tags:
  - cpp/classes
  - cpp/constructors
  - concept
  - syntax
  - best-practice
aliases:
  - std::initializer_list
  - initializer_list
  - list constructor
  - initializer list constructor
up: "[[Chapter 23 — Object Relationships]]"
related:
  - "[[Container Classes]]"
  - "[[Constructors]]"
  - "[[List Initialization]]"
  - "[[Delegating Constructors]]"
  - "[[Overloading the Assignment Operator]]"
  - "[[static_cast]]"
  - "[[Chapter 23 — Object Relationships]]"
---

# std::initializer_list

`std::initializer_list<T>` (in `<initializer_list>`) is a lightweight view the compiler creates automatically when it sees a brace-enclosed list used to initialize an object. Writing a constructor that accepts `std::initializer_list` lets a class be initialized with `{ val1, val2, ... }` syntax.

## How it works

When the compiler sees:

```cpp
IntArray array{ 5, 4, 3, 2, 1 };
```

it silently constructs a `std::initializer_list<int>` from the braces and passes it to the matching constructor. Use [[Delegating Constructors|delegating constructors]] to avoid duplicating setup code:

```cpp
IntArray(std::initializer_list<int> list)
    : IntArray(static_cast<int>(list.size())) // delegate to size constructor
{
    std::copy(list.begin(), list.end(), m_data);
}
```

`static_cast` is used here to convert `size_t` to `int` — see [[static_cast]].

## Iterating the list

`std::initializer_list` does **not** support subscript (`operator[]`). Use a range-based for loop or the `begin()`/`end()` iterators:

```cpp
for (auto val : list)
    // process val
```

## List constructor priority

When [[List Initialization]] (`{}`) is used, the compiler **prefers list constructors** over other constructors — even if a non-list constructor would otherwise be a better match. This is a subtle source of bugs when a list constructor is added to an existing class.

```cpp
IntArray a(5);    // calls IntArray(int) — 5-element array
IntArray b{ 5 };  // calls IntArray(initializer_list) — 1-element array containing 5
```

**Adding a list constructor to an existing class that did not have one may silently change the behavior of existing `{}`-initialized calls.**

## List assignment

If you provide a list constructor, also provide a **list assignment operator** so that `obj = { ... }` is consistent with construction. Options:

1. Overload `operator=` to accept `std::initializer_list<T>`.
2. Provide a proper deep-copying copy assignment operator.
3. Delete copy assignment if assignment should only work via initializer list.

> Full coverage: [[Chapter 23 — Object Relationships]] → std::initializer_list
