---
tags:
  - cpp/arrays
  - cpp/pointers
  - cpp/c-style
  - concept
  - subnode
aliases:
  - array decay
  - array-to-pointer conversion
  - decay
  - pointer decay
up: "[[C-style Arrays]]"
related:
  - "[[C-style Arrays]]"
  - "[[Pointers]]"
  - "[[Pass by Address]]"
  - "[[Memory Model]]"
  - "[[Undefined Behavior]]"
---

# Array Decay

**Array decay** is the implicit conversion of a C-style array to a pointer to its first element. It occurs in most expression contexts and is a major source of C-style array pitfalls.

```cpp
int arr[] { 1, 2, 3 };
int* ptr { arr };       // arr decays to int* pointing to arr[0]
```

The decayed pointer carries **no length information** — the array's size is lost.

## When decay does NOT occur

A C-style array retains its array type (no decay) in four situations:

| Context | Example |
|---|---|
| Argument to `sizeof` or `typeid` | `sizeof(arr)` → total bytes, not pointer size |
| Address-of operator `&` | `&arr` → pointer to the whole array, type `int(*)[3]` |
| Passed as a class member | Aggregate member stays as array type |
| Passed by reference | `void f(int (&arr)[3])` receives the array, not a pointer |

## Pass by address (looks like pass by value)

C-style arrays **always** pass by address — even when the syntax looks like pass by value:

```cpp
void f(int arr[]);  // equivalent to: void f(int* arr)
```

The array type in a parameter declaration is adjusted to a pointer. Use array syntax (`int arr[]`) rather than pointer syntax (`int* arr`) to make intent clearer, but both are identical to the compiler.

## Length information is lost

Arrays with the same element type but different lengths decay to the same pointer type:

```cpp
int a[3];
int b[5];
// both decay to int*
```

This means you must pass the length separately when working with decayed arrays.

## Effect on range-based for

Range-based for loops require a non-decayed array (or an object with `begin()`/`end()`). A decayed pointer cannot be used directly in a range-based for loop.

See also [[Pass by Address]] for address-passing semantics, and [[Pointers]] for pointer fundamentals.

> Full coverage: [[Chapter 17 — Dynamic memory allocation]] → C-style Arrays
