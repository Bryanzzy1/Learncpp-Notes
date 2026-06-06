---
tags:
  - cpp/pointers
  - cpp/arrays
  - cpp/memory
  - concept
  - syntax
  - subnode
aliases:
  - 2D dynamic array
  - two-dimensional array
  - jagged array
up: "[[Pointers to Pointers]]"
related:
  - "[[Pointers to Pointers]]"
  - "[[Dynamic Array Allocation]]"
  - "[[new and delete]]"
  - "[[Memory Leaks]]"
  - "[[std-vector|std::vector]]"
  - "[[Undefined Behavior]]"
---

# Two-Dimensional Dynamic Arrays

A 2D dynamic array is built as an `int**`: an outer array of row pointers, where each row pointer points to its own heap-allocated column array.

## Allocation

```cpp
int** array{ new int*[10] };              // 10 row pointers
for (int count{ 0 }; count < 10; ++count)
    array[count] = new int[5]{};          // each row has 5 columns
```

Access syntax is identical to a fixed 2D array: `array[row][col]`.

## Deallocation (order matters)

```cpp
for (int count{ 0 }; count < 10; ++count)
    delete[] array[count];   // free each row first
delete[] array;              // then free the outer pointer array
```

Freeing the outer array first makes the inner arrays unreachable — a [[Memory Leaks|memory leak]]. Accessing the inner arrays after freeing the outer array is [[Undefined Behavior]].

## Comparison: raw `int**` vs std::vector

| Aspect | `int**` (pointer-to-pointer) | `std::vector<std::vector<T>>` |
|---|---|---|
| Row lengths | Can vary (jagged) | Can vary |
| Memory layout | Non-contiguous | Non-contiguous |
| Bounds checking | None | Yes (`.at()`) |
| Manual memory management | Yes | No ([[RAII]]) |

Prefer `std::vector<std::vector<T>>` or a flat [[std-vector|std::vector]] with manual index math over raw `int**`.

> Full coverage: [[Chapter 19 — Dynamic Allocation]] → Two-Dimensional Dynamic Arrays
