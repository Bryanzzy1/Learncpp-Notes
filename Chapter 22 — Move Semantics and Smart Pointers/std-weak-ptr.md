---
tags:
  - cpp/memory
  - cpp/pointers
  - concept
  - syntax
  - subnode
aliases:
  - std::weak_ptr
  - weak_ptr
  - circular reference
  - cyclical reference
  - non-owning observer
  - expired()
up: "[[std-shared-ptr|std::shared_ptr]]"
related:
  - "[[std-shared-ptr|std::shared_ptr]]"
  - "[[Smart Pointers]]"
  - "[[Memory Leaks]]"
  - "[[Undefined Behavior]]"
  - "[[Chapter 22 — Move Semantics and Smart Pointers]]"
---

# std::weak_ptr

`std::weak_ptr` (in `<memory>`, C++11) is a **non-owning observer** of a [[std-shared-ptr|std::shared_ptr]]-managed resource. It holds a reference to the resource without incrementing the reference count, which is the key to breaking **circular references**.

## The circular reference problem

A **circular reference** (cyclical reference) occurs when two or more objects each hold a `shared_ptr` to each other (directly or through a chain), preventing the reference count of any of them from ever reaching zero:

```cpp
struct Node {
    std::shared_ptr<Node> next;
    std::shared_ptr<Node> prev; // if prev also points back → cycle
};
```

Even a single object can create a cycle:

```cpp
auto ptr1 = std::make_shared<Resource>();
ptr1->m_ptr = ptr1; // m_ptr is a shared_ptr<Resource> — cycle: ptr1 refers to itself
```

In both cases the reference count never drops to zero → [[Memory Leaks|memory leak]].

## Solution: replace one side of the cycle with weak_ptr

Replace owning `shared_ptr` members on the "back" side of the relationship with `weak_ptr`:

```cpp
struct Node {
    std::shared_ptr<Node> next;  // owning
    std::weak_ptr<Node>   prev;  // non-owning — breaks the cycle
};
```

Now `prev` does not contribute to the reference count. When the last real `shared_ptr` to a Node is destroyed, the Node is freed even if a `weak_ptr` still references it.

## Using a weak_ptr

`weak_ptr` has no `operator*` or `operator->`. To access the managed object, first **lock** it into a temporary `shared_ptr`:

```cpp
std::weak_ptr<Resource> weak = shared;

if (auto locked = weak.lock()) // returns shared_ptr; null if object is gone
{
    locked->doSomething();
} // locked is destroyed here; reference count drops back
```

## Checking validity

Use `expired()` to test whether the observed resource is still alive without obtaining ownership:

```cpp
if (weak.expired())
    // object has been destroyed
```

`expired()` returns `true` if the `weak_ptr` points to an object that has already been deallocated.

> Full coverage: [[Chapter 22 — Move Semantics and Smart Pointers]] → std::shared_ptr → std::weak_ptr
