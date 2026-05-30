---
tags:
  - cpp/arrays
  - cpp/containers
  - concept
aliases:
  - container
  - array
  - homogenous container
  - container class
up: "[[Chapter 16 — Arrays, Strings, Pointers]]"
related:
  - "[[std::vector]]"
  - "[[Memory Model]]"
  - "[[Initialization]]"
---

# Containers and Arrays

A **container** is a data type that provides storage for a collection of unnamed objects called **elements**. In C++ (and most programming languages), containers are **homogenous** — all elements must have the same type.

The term **length** refers to the number of elements currently in a container. The term **size** refers to the amount of storage (in bytes) the container object occupies. These are distinct.

## Container classes

A class type that implements a container is a **container class**. Examples:

- C-style arrays
- `std::string`
- `std::vector`
- `std::array`

## Arrays

An **array** is a container that stores elements **contiguously** in [[Memory Model|memory]] — each element occupies an adjacent memory location with no gaps between them.

This contiguous layout enables **random access**: any element can be reached directly via its index, without traversing earlier elements (unlike sequential-access containers such as linked lists).

C++ provides three primary array types:

| Type | Key trait |
|------|-----------|
| C-style array | Fixed size, decays to pointer |
| `std::vector` | Dynamic size, heap-allocated |
| `std::array` | Fixed size, stack-allocated, `constexpr`-capable |

> Full coverage: [[Chapter 16 — Arrays, Strings, Pointers]] → Containers and Arrays
