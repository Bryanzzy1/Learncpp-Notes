---
tags:
  - cpp/memory
  - cpp/arrays
  - concept
  - syntax
  - best-practice
aliases:
  - array new
  - array delete
  - new[]
  - delete[]
  - dynamic array
up: "[[Chapter 19 — Dynamic Allocation]]"
related:
  - "[[new and delete]]"
  - "[[Memory Leaks]]"
  - "[[C-style Arrays]]"
  - "[[Array Decay]]"
  - "[[Pointers]]"
  - "[[std-vector|std::vector]]"
  - "[[Undefined Behavior]]"
---

# Dynamic Array Allocation

Use the **array form** of `new` and `delete` to allocate arrays whose length is not known until runtime:

```cpp
int* array{ new int[length]{} };  // length can be a runtime variable
delete[] array;                    // must use delete[], not delete
```

The `{}` value-initializes all elements (to zero for `int`).

## Matching operators

| Situation | Allocate | Free |
|---|---|---|
| Single object | `new T` | `delete ptr` |
| Array | `new T[n]` | `delete[] ptr` |

Mismatching these (e.g. `delete` on a `new[]` block) is [[Undefined Behavior]].

## Runtime-length advantage

Unlike [[C-style Arrays]], which require a compile-time constant length, dynamically allocated arrays accept a runtime value:

```cpp
int length{};
std::cin >> length;
int* arr{ new int[length]{} };   // valid: length determined at runtime
```

## Dynamic arrays vs fixed arrays

A dynamically allocated array and a fixed-size [[C-style Arrays|C-style array]] behave almost identically after allocation — both decay to a pointer ([[Array Decay]]) and use the same `[]` index syntax. The key difference is who manages lifetime: you do with `new[]`/`delete[]`, the runtime does with a fixed array on the stack.

## Prefer std::vector

Raw dynamic arrays are error-prone: they decay to pointers, must be manually freed, and offer no bounds checking. Use [[std-vector|std::vector]] instead — it wraps a dynamic array with automatic memory management ([[RAII]]) and a richer interface.

> Full coverage: [[Chapter 19 — Dynamic Allocation]] → Dynamic Array Allocation
