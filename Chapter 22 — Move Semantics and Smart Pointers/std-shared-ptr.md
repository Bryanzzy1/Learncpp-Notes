---
tags:
  - cpp/memory
  - cpp/pointers
  - concept
  - syntax
  - best-practice
aliases:
  - std::shared_ptr
  - shared_ptr
  - shared ownership pointer
  - reference counting
  - std::make_shared
  - make_shared
up: "[[Chapter 22 — Move Semantics and Smart Pointers]]"
related:
  - "[[Smart Pointers]]"
  - "[[RAII]]"
  - "[[new and delete]]"
  - "[[Memory Leaks]]"
  - "[[std-unique-ptr|std::unique_ptr]]"
  - "[[std-weak-ptr|std::weak_ptr]]"
  - "[[Undefined Behavior]]"
  - "[[Chapter 22 — Move Semantics and Smart Pointers]]"
---

# std::shared_ptr

`std::shared_ptr` (in `<memory>`, C++11) is the standard [[Smart Pointers|smart pointer]] for **shared ownership**: multiple `shared_ptr` instances can co-own the same resource. Internally it maintains a **reference count** — the number of `shared_ptr` objects pointing to the resource. The resource is deallocated when the last owning `shared_ptr` is destroyed or reset.

## Creating a shared_ptr

**Prefer `std::make_shared` (C++11) over direct construction:**

```cpp
auto ptr = std::make_shared<Resource>(); // single allocation for object + control block
```

Direct construction with `new` requires two allocations (object + control block separately) and is less safe:

```cpp
std::shared_ptr<Resource> ptr{ new Resource{} }; // less preferred
```

## Copying vs constructing from raw pointer

To have multiple `shared_ptr` pointing to the same resource, **always copy an existing `shared_ptr`** — never construct a new one from the same raw pointer:

```cpp
auto p1 = std::make_shared<Resource>();
auto p2 = p1; // correct — p2 is a copy; reference count is now 2

Resource* raw = new Resource{};
std::shared_ptr<Resource> a{ raw };
std::shared_ptr<Resource> b{ raw }; // WRONG — two separate ref counts → double-free
```

## Creating a shared_ptr from a unique_ptr

A `std::unique_ptr` can be converted to a `std::shared_ptr` (the reverse is not possible):

```cpp
auto uniq = std::make_unique<Resource>();
std::shared_ptr<Resource> shared = std::move(uniq); // unique_ptr yields ownership
```

## Array support (C++20)

As of C++20, `std::shared_ptr` supports arrays. Prefer `std::vector` for most array use cases.

## Reference count lifecycle

The resource is freed when the **last** `shared_ptr` managing it is destroyed. Individual `shared_ptr` instances can be destroyed independently, and the resource survives until the count drops to zero.

## Pitfall: circular references

When two objects each hold a `shared_ptr` to the other, neither reference count ever reaches zero — a memory leak. See [[std-weak-ptr|std::weak_ptr]] for the solution.

> Full coverage: [[Chapter 22 — Move Semantics and Smart Pointers]] → std::shared_ptr
