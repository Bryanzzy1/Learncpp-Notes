---
tags:
  - cpp/pointers
  - cpp/arrays
  - concept
  - syntax
aliases:
  - pointer arithmetic
  - subscript operator pointer
  - pointer addition
  - pointer subtraction
up: "[[Chapter 17 — Dynamic memory allocation]]"
related:
  - "[[Pointers]]"
  - "[[Memory Model]]"
  - "[[C-style Arrays]]"
  - "[[Array Decay]]"
---

# Pointer Arithmetic

**Pointer arithmetic** applies integer arithmetic operators (+, −, ++, −−) to a pointer to produce a new memory address. The result is scaled by the **size of the pointed-to type**, not by bytes.

```cpp
int arr[] { 10, 20, 30, 40 };
int* ptr { arr };        // points to arr[0]

ptr + 1;                 // address of arr[1] (not ptr + 1 byte)
ptr + 3;                 // address of arr[3]
```

Given `ptr` pointing to a `T`, `ptr + n` returns the address of the object that is `n` positions further in [[Memory Model|memory]] — equivalent to `n * sizeof(T)` bytes ahead.

## Subscripting is pointer arithmetic

The subscript operator `arr[n]` is defined as syntactic sugar for pointer arithmetic:

```cpp
arr[n]  ==  *((arr) + (n))
```

Because addition is commutative, `n[arr]` is also valid (though unusual):

```cpp
3[arr]  ==  *(arr + 3)  ==  arr[3]  // all equivalent
```

## Addresses are relative

Pointer arithmetic produces **relative** addresses — the result is meaningful only within the bounds of the same array (or one-past-the-end). Arithmetic that goes out of bounds produces [[Undefined Behavior|undefined behavior]].

Pointer arithmetic is the mechanism by which C-style array indexing and iteration work at the machine level. `std::array` and `std::vector` index their elements the same way internally; they just provide a safer interface on top. See [[Pointers]] for pointer fundamentals and [[Array Decay]] for how arrays become pointers.

> Full coverage: [[Chapter 17 — Dynamic memory allocation]] → Pointer arithmetic and subscripting
