---
tags:
  - cpp/pointers
  - cpp/types
  - concept
  - syntax
aliases:
  - void*
  - generic pointer
  - void pointer
up: "[[Chapter 19 — Dynamic Allocation]]"
related:
  - "[[Pointers]]"
  - "[[static_cast]]"
  - "[[Function Templates]]"
  - "[[Memory Model]]"
---

# Void Pointers

A **void pointer** (`void*`) is the generic pointer type — it can hold the address of any object type. Because it carries no type information, it cannot be dereferenced directly; it must first be cast to a concrete pointer type via [[static_cast]]:

```cpp
int i{ 5 };
void* voidPtr{ &i };                          // any address is valid

int* intPtr{ static_cast<int*>(voidPtr) };    // cast required before deref
std::cout << *intPtr << '\n';
```

## Restrictions

| Operation | Allowed? |
|---|---|
| Store any address | Yes |
| Dereference directly | **No** — must cast first |
| Pointer arithmetic | **No** — element size is unknown |
| `delete` directly | **Avoid** — [[static_cast]] back to the real type first |

## Why templates are better

`void*` was the C-era solution for type-generic code. In C++, [[Function Templates]] provide the same generality with **compile-time type safety** — each instantiation knows the concrete type. Prefer templates over `void*` in new code.

> Full coverage: [[Chapter 19 — Dynamic Allocation]] → Void Pointers
