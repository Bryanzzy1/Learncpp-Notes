---
tags:
  - cpp/memory
  - cpp/classes
  - concept
  - syntax
  - best-practice
aliases:
  - move semantics
  - move ownership
  - move-enabled class
  - transfer ownership
up: "[[Chapter 22 — Move Semantics and Smart Pointers]]"
related:
  - "[[Move Constructor and Assignment]]"
  - "[[Copy Constructor]]"
  - "[[Rvalue References]]"
  - "[[std-move|std::move]]"
  - "[[Value Categories]]"
  - "[[Shallow vs Deep Copying]]"
  - "[[Overloading the Assignment Operator]]"
  - "[[Chapter 22 — Move Semantics and Smart Pointers]]"
---

# Move Semantics

**Move semantics** is a C++11 feature that allows a class to **transfer ownership** of its managed resource to another object rather than making an expensive copy. Move semantics is an optimization: instead of duplicating heap memory, you reassign a pointer and null out the source.

## Move vs copy

| | Copy | Move |
|---|---|---|
| Source state after | Unchanged (full duplicate exists) | Valid but unspecified (resource transferred) |
| Cost | O(n) for heap data | O(1) — pointer swap |
| Invoked when | Argument is an lvalue | Argument is an rvalue (temporary) |

Compare with [[Shallow vs Deep Copying]]: a shallow copy copies the pointer value (dangerous double-free); a deep copy duplicates data (expensive); a move transfers ownership (cheap and safe).

## When move semantics apply

The compiler invokes move semantics when the source is an **rvalue** (a temporary or an object explicitly cast via [[std-move|std::move]]). See [[Value Categories]] for the distinction between lvalues and rvalues.

## Pre-C++11 workaround

Before C++11, move had to be faked through a non-const copy constructor that stole the source's pointer:

```cpp
Auto_ptr2(Auto_ptr2& a) // non-const — signals intent to mutate source
{
    m_ptr = a.m_ptr;
    a.m_ptr = nullptr;
}
```

In C++11 this is expressed cleanly with [[Rvalue References]] (`&&`) and [[Move Constructor and Assignment|move constructor/assignment]].

## Move functions must leave the source in a valid state

After a move, the source object must remain destructible and assignable — it just no longer owns the resource. The standard idiom is to null the source's pointer after the transfer.

## Disabling copying in move-enabled classes

When a class is designed to be moved but not copied, delete the copy operations:

```cpp
MyClass(const MyClass&) = delete;
MyClass& operator=(const MyClass&) = delete;
```

The [[Copy Constructor|copy constructor]] and copy assignment can be `= delete`d the same way as move operations. Deleting move operations (via `= delete`) prevents implicit move constructor generation and makes the class non-returnable by value when copy elision does not apply.

## Pitfall: std::swap and move semantics

Do **not** implement the move constructor or move assignment by calling `std::swap()`. `std::swap` itself calls move operations on move-capable types, which causes infinite recursion.

> Full coverage: [[Chapter 22 — Move Semantics and Smart Pointers]] → Move Semantics
