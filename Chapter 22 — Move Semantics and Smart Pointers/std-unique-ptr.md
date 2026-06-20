---
tags:
  - cpp/memory
  - cpp/pointers
  - concept
  - syntax
  - best-practice
aliases:
  - std::unique_ptr
  - unique_ptr
  - sole ownership pointer
  - std::make_unique
  - make_unique
up: "[[Chapter 22 — Move Semantics and Smart Pointers]]"
related:
  - "[[Smart Pointers]]"
  - "[[RAII]]"
  - "[[new and delete]]"
  - "[[Memory Leaks]]"
  - "[[std-move|std::move]]"
  - "[[std-shared-ptr|std::shared_ptr]]"
  - "[[Move Semantics]]"
  - "[[Undefined Behavior]]"
  - "[[Chapter 22 — Move Semantics and Smart Pointers]]"
---

# std::unique_ptr

`std::unique_ptr` (in `<memory>`, C++11) is the standard [[Smart Pointers|smart pointer]] for **sole ownership**: exactly one `unique_ptr` owns the resource at a time. When the `unique_ptr` is destroyed, it automatically calls `delete` on the managed object — the [[RAII]] pattern applied to heap memory.

It replaces the deprecated `std::auto_ptr`.

## Creating a unique_ptr

**Prefer `std::make_unique` (C++14) over direct construction:**

```cpp
auto res = std::make_unique<Resource>();       // no raw new needed
auto arr = std::make_unique<int[]>(10);        // array version
```

`std::make_unique` constructs the object and wraps it in a single expression, avoiding potential resource leaks from evaluation-order issues with `new`.

Direct construction is also possible but less preferred:

```cpp
std::unique_ptr<Resource> res{ new Resource{} };
```

## Accessing the managed object

`unique_ptr` overloads `operator*` (returns a reference) and `operator->` (returns a pointer) for natural use:

```cpp
auto res = std::make_unique<Resource>();
res->doSomething();  // operator->
(*res).doSomething(); // operator*
```

## Move-only semantics

`unique_ptr` has copy semantics **disabled** — only move is allowed. To transfer ownership, use [[std-move|std::move]]:

```cpp
auto a = std::make_unique<Resource>();
auto b = std::move(a); // a is now null; b owns the resource
```

To pass a `unique_ptr` to a function that takes ownership, you must `std::move` the argument:

```cpp
void consume(std::unique_ptr<Resource> r);
consume(std::move(res)); // transfers ownership into the function
```

## Return from functions

Return `unique_ptr` by value — the compiler applies move semantics (or copy elision) automatically. Do **not** return by pointer or reference.

## Common pitfalls

```cpp
// DON'T: two unique_ptrs managing the same raw pointer → double-free (Undefined Behavior)
Resource* raw = new Resource{};
std::unique_ptr<Resource> p1{ raw };
std::unique_ptr<Resource> p2{ raw };  // WRONG

// DON'T: manually delete the resource — unique_ptr will delete it again
delete raw; // WRONG
```

## Prefer standard containers over unique_ptr for arrays

Favor `std::array`, `std::vector`, or `std::string` over a `unique_ptr` managing a fixed or dynamic array.

> Full coverage: [[Chapter 22 — Move Semantics and Smart Pointers]] → std::unique_ptr
