---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - syntax
aliases:
  - this
  - this pointer
  - implicit object pointer
up: "[[Chapter 15 — More on Classes]]"
related:
  - "[[Member Function Chaining]]"
  - "[[Static Member Functions]]"
  - "[[Member Functions]]"
  - "[[Const Member Functions]]"
  - "[[Classes]]"
  - "[[Pointers]]"
---

# this Pointer

Inside every non-static member function, **`this`** is an implicitly available const pointer that holds the address of the implicit object — the object the function was called on.

```cpp
simple.setID(2);
// The compiler rewrites this as something like:
Simple::setID(&simple, 2);

// and the member function receives:
void setID(Simple* const this, int id) { this->m_id = id; }
```

`this` always points to the object being operated on — it cannot be reseated.

## Type of `this`

| Context | Type of `this` |
|---------|---------------|
| Non-const member function | `ClassName* const` (const pointer, mutable object) |
| Const member function | `const ClassName* const` (const pointer, const object) |

In a [[Const Member Functions|const member function]], `this` is a pointer to a `const` object, so neither the pointer nor the object it points to can be modified.

## Explicit use of `this->`

While member accesses inside a member function implicitly go through `this`, you can use `this->member` explicitly. This is most useful for disambiguation when a parameter name shadows a member name.

## Subnodes

- [[Member Function Chaining]] — returning `*this` to enable chained calls

## Static member functions

[[Static Member Functions]] have **no `this` pointer** — they are not associated with any object.

> Full coverage: [[Chapter 15 — More on Classes]] → this Pointer
