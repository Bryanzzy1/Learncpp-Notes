---
tags:
  - cpp/memory
  - cpp/pointers
  - concept
  - syntax
  - best-practice
aliases:
  - operator new
  - operator delete
  - heap allocation
  - dynamic allocation
  - new
  - delete
up: "[[Chapter 19 — Dynamic Allocation]]"
related:
  - "[[Memory Leaks]]"
  - "[[Dynamic Array Allocation]]"
  - "[[RAII]]"
  - "[[Pointers]]"
  - "[[Memory Model]]"
  - "[[Undefined Behavior]]"
  - "[[std-vector|std::vector]]"
---

# new and delete

C++ offers three types of memory allocation:

| Type | When allocated | When freed | Size fixed at compile time? |
|---|---|---|---|
| **Static** | Program start | Program end | Yes |
| **Automatic** | Block entry | Block exit | Yes |
| **Dynamic** | Explicitly via `new` | Explicitly via `delete` | No |

Dynamic allocation puts objects on the **heap**, making size and lifetime programmable at runtime. In exchange, the program — not the runtime — is responsible for freeing that memory. Heap access is generally slower than stack access (see [[Memory Model]]).

## Allocating a single variable

```cpp
int* ptr{ new int };        // allocates an int; ptr holds its address
int* ptr2{ new int{5} };    // allocates and value-initializes to 5
```

`new` returns a pointer to the allocated object.

## Operator new can fail

If the OS cannot satisfy the request, `new` throws `std::bad_alloc` by default. The `std::nothrow` variant returns `nullptr` instead:

```cpp
int* value{ new (std::nothrow) int };
if (value == nullptr)
    ; // allocation failed — handle gracefully
```

## Freeing memory

Use the scalar `delete` when done:

```cpp
delete ptr;      // return memory to OS
ptr = nullptr;   // prevent dangling pointer
```

- Deleting a `nullptr` is a no-op — no need to guard with an `if`.
- Set the pointer to `nullptr` after deletion unless it immediately goes out of scope; otherwise dereferencing it is [[Undefined Behavior]].
- Never `delete` a pointer that was not returned by `new`.

## Stack vs heap

Stack allocation and deallocation is automatic (managed by the compiler). Heap memory must be managed manually — this is why [[RAII]]-based wrappers like [[std-vector|std::vector]] are preferred over raw `new`/`delete` in modern C++. Losing the only pointer to a heap block before calling `delete` creates a [[Memory Leaks|memory leak]].

> Full coverage: [[Chapter 19 — Dynamic Allocation]] → new and delete
