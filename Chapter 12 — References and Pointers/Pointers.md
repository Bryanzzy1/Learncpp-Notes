---
tags:
  - cpp/pointers
  - cpp/memory
  - concept
  - syntax
  - best-practice
aliases:
  - pointer
  - raw pointer
  - null pointer
  - nullptr
  - dereference operator
  - address-of operator
  - pointer type
  - const pointer
  - pointer to const
up: "[[Chapter 12 — References and Pointers]]"
related:
  - "[[Pass by Address]]"
  - "[[In and Out Parameters]]"
  - "[[Lvalue References]]"
  - "[[Value Categories]]"
  - "[[Type Deduction with References and Pointers]]"
  - "[[Memory Model]]"
  - "[[Undefined Behavior]]"
  - "[[Initialization]]"
---

# Pointers

A **pointer** is an object that holds a **memory address** as its value (typically the address of another variable). Modern C++ calls these **raw pointers** to distinguish them from smart pointers.

## Operators

- **Address-of operator** (`&`) — returns the memory address of its operand as an rvalue.
- **Dereference operator** (`*`) (also called the **indirection operator**) — returns the value at a given memory address as an lvalue.

## Declaration syntax

```cpp
int *ptr;             // pointer to int
const int *ptr;       // pointer to const int
int *const ptr;       // const pointer to int
const int *const ptr; // const pointer to const int
```

The pointer type (e.g. `int*`) encodes what type of object the pointer addresses.

## Size

Pointer size depends on the target architecture: 32-bit executables use 32-bit (4-byte) pointers; 64-bit executables use 64-bit (8-byte) pointers. See [[Memory Model]].

## Always initialize your pointers

Uninitialized pointers hold garbage addresses. Value-initialize to `nullptr` if you have no valid address yet:

```cpp
int* ptr { nullptr };
```

`nullptr` (type `std::nullptr_t`, defined in `<cstddef>`) represents a null pointer. **Dereferencing a null pointer is [[Undefined Behavior]].**

Check before use: `if (ptr != nullptr)` or the implicit bool form `if (ptr)`.

A pointer should always either hold the address of a valid object **or** be set to `nullptr`.

## Pointer to const vs. const pointer

| Declaration | What's const |
| --- | --- |
| `const int* ptr` | The *value* pointed to (pointer can be reassigned) |
| `int* const ptr` | The *address* in the pointer (can't point elsewhere after init) |
| `const int* const ptr` | Both |

A pointer to const treats the pointed-to value as constant regardless of whether the actual object is const.

## Pointers vs. references

| Property | References | Pointers |
| --- | --- | --- |
| Is an object | No | Yes |
| Can be null | No | Yes |
| Can be reassigned | No (can't reseat) | Yes |
| Safety | Safe (except dangling) | Inherently dangerous |

**Favor [[Lvalue References|references]] over pointers** whenever possible. Use pointers when you need nullable semantics or need to change what is pointed to.

For passing addresses to functions, see [[Pass by Address]]. For type deduction interactions, see [[Type Deduction with References and Pointers]].

> Full coverage: [[Chapter 12 — References and Pointers]] → Pointers
