---
tags:
  - cpp/pointers
  - cpp/memory
  - concept
  - syntax
aliases:
  - pointer to pointer
  - double pointer
  - int**
up: "[[Chapter 19 — Dynamic Allocation]]"
related:
  - "[[Two-Dimensional Dynamic Arrays]]"
  - "[[Pointers]]"
  - "[[Dynamic Array Allocation]]"
  - "[[new and delete]]"
  - "[[Memory Model]]"
---

# Pointers to Pointers

A **pointer to a pointer** stores the address of another pointer rather than the address of a data object:

```cpp
int** ptrptr;  // pointer to a pointer to an int
```

Dereferencing once (`*ptrptr`) yields a `int*`. Dereferencing twice (`**ptrptr`) yields the final `int` value.

## Subnodes

- [[Two-Dimensional Dynamic Arrays]] — the primary use case: building a dynamically allocated 2D grid

## Primary use: array of pointers

The most common use is to dynamically allocate an **array of pointers**, where each element then points to its own heap array — giving a 2D structure with runtime dimensions:

```cpp
int** grid{ new int*[rows] };          // array of row pointers
for (int i{ 0 }; i < rows; ++i)
    grid[i] = new int[cols]{};         // each row is a separate heap array
```

Deallocation must free each inner array before the outer array:

```cpp
for (int i{ 0 }; i < rows; ++i)
    delete[] grid[i];
delete[] grid;
```

This is still just [[Pointers]] — `int**` is a pointer whose pointee happens to be another pointer. The mechanics of [[Dynamic Array Allocation]] apply at every level of indirection.

> Full coverage: [[Chapter 19 — Dynamic Allocation]] → Pointers to Pointers
