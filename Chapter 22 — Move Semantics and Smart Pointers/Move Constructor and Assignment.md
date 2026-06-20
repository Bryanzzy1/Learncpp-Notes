---
tags:
  - cpp/memory
  - cpp/classes
  - concept
  - syntax
  - subnode
aliases:
  - move constructor
  - move assignment operator
  - move assignment
  - move-enabled type
  - implicit move constructor
  - noexcept move
up: "[[Move Semantics]]"
related:
  - "[[Move Semantics]]"
  - "[[Copy Constructor]]"
  - "[[Rvalue References]]"
  - "[[Overloading the Assignment Operator]]"
  - "[[Value Categories]]"
  - "[[Chapter 22 — Move Semantics and Smart Pointers]]"
---

# Move Constructor and Assignment

The **move constructor** and **move assignment operator** are special member functions (C++11) that transfer ownership of a resource from a source object to a new or existing object. They are invoked when the source is an **rvalue** — see [[Rvalue References]] and [[Value Categories]].

## Signatures

Both take an rvalue reference (`&&`) parameter and should be marked `noexcept`:

```cpp
// Move constructor
Auto_ptr4(Auto_ptr4&& a) noexcept
    : m_ptr { a.m_ptr }
{
    a.m_ptr = nullptr; // source must remain in a valid (destructible) state
}

// Move assignment operator
Auto_ptr4& operator=(Auto_ptr4&& a) noexcept
{
    if (&a == this)     // self-assignment guard
        return *this;

    delete m_ptr;       // release current resource

    m_ptr = a.m_ptr;    // transfer ownership
    a.m_ptr = nullptr;

    return *this;
}
```

## Why noexcept matters

Marking move operations `noexcept` is important for performance: standard library containers (e.g. `std::vector`) will only use move semantics during reallocation if the move constructor is `noexcept`; otherwise they fall back to copying for exception safety.

## When each is called

| Situation | Function called |
|---|---|
| `T obj { std::move(other) };` | Move constructor |
| `obj = std::move(other);` | Move assignment |
| Source is a temporary rvalue | Move constructor or move assignment (if defined) |
| Source is an lvalue (or move not available) | [[Copy Constructor]] or copy assignment |

The [[Copy Constructor|copy]] versions are used as fallback when the argument is an lvalue or when no move version is defined.

## Implicit move constructor and move assignment

The compiler generates implicit move operations if **all** of the following hold:

- No user-declared copy constructor or copy assignment operator.
- No user-declared move constructor or move assignment operator.
- No user-declared destructor.

Following the **rule of five** (extension of the [[Copy Constructor|rule of three]]): if a class defines any of copy constructor, copy assignment, move constructor, move assignment, or destructor, consider explicitly defining or `= default`-ing all five.

> Full coverage: [[Chapter 22 — Move Semantics and Smart Pointers]] → Move Semantics → Move Constructor and Assignment
