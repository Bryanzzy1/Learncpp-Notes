---
tags:
  - cpp/memory
  - cpp/classes
  - concept
  - best-practice
  - subnode
aliases:
  - shallow copy
  - deep copy
  - memberwise copy
  - shallow vs deep copy
up: "[[Overloading the Assignment Operator]]"
related:
  - "[[Overloading the Assignment Operator]]"
  - "[[Copy Constructor]]"
  - "[[new and delete]]"
  - "[[Memory Leaks]]"
  - "[[Undefined Behavior]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Shallow vs Deep Copying

## Shallow copy (memberwise copy)

The compiler's implicit [[Copy Constructor|copy constructor]] and [[Overloading the Assignment Operator|copy assignment operator]] perform a **shallow copy** — each member is copied directly from source to destination.

For classes that own heap-allocated memory, this copies the **pointer value**, not the memory it points to. Both the source and destination then share the same allocation:

```
Source:  m_data ──► [ 1, 2, 3 ]
Copy:    m_data ──► [ 1, 2, 3 ]  ← same memory!
```

Problems that follow:
- Modifying one object's data modifies the other.
- When either object is destroyed, it frees the shared memory — the surviving object now holds a dangling pointer ([[Undefined Behavior]]).
- If both objects are destroyed, the memory is freed twice (double-free — [[Undefined Behavior]]).

## Deep copy

A **deep copy** allocates a new block of memory and copies the pointed-to data into it. Each object owns its own allocation:

```
Source:  m_data ──► [ 1, 2, 3 ]
Copy:    m_data ──► [ 1, 2, 3 ]  ← separate allocation
```

```cpp
MyClass& MyClass::operator=(const MyClass& source)
{
    if (this == &source)   // self-assignment guard
        return *this;

    delete[] m_data;       // free current allocation
    m_data = new int[source.m_size];           // allocate new
    std::copy(source.m_data, source.m_data + source.m_size, m_data); // copy data
    m_size = source.m_size;
    return *this;
}
```

## When each applies

| Class has... | Default behavior sufficient? |
|---|---|
| No pointers / only value members | Yes — memberwise copy is correct |
| Owning raw pointer (`new`/`delete`) | No — must write deep copy |
| Smart pointer (`std::unique_ptr`, etc.) | Usually no — smart pointers handle ownership, but you may still need custom logic |

When you write a deep-copy assignment operator, follow the **rule of three**: also write a custom copy constructor and destructor (see [[Copy Constructor]]). Failing to do so leaves one path using shallow copy and another using deep copy, which causes subtle bugs and [[Memory Leaks]].

> Full coverage: [[Chapter 21 — Operator Overloading]] → Overloading the Assignment Operator → Shallow vs Deep Copying
