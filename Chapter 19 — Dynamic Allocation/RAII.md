---
tags:
  - cpp/memory
  - cpp/classes
  - concept
  - best-practice
aliases:
  - Resource Acquisition Is Initialization
  - resource management
  - destructor cleanup
up: "[[Chapter 19 — Dynamic Allocation]]"
related:
  - "[[Destructors]]"
  - "[[Constructors]]"
  - "[[new and delete]]"
  - "[[Memory Leaks]]"
  - "[[Classes]]"
  - "[[std-vector|std::vector]]"
---

# RAII

**RAII** (Resource Acquisition Is Initialization) is a programming pattern where a resource is acquired in an object's [[Constructors|constructor]] and released in its [[Destructors|destructor]]. Because the destructor runs automatically when the object's lifetime ends, the resource is always freed — even if an exception is thrown.

## Pattern: destructor-managed dynamic memory

```cpp
class IntArray
{
    int* m_array{};
public:
    IntArray(int length)
        : m_array{ new int[length]{} }   // acquire in constructor
    {}

    ~IntArray()
    {
        delete[] m_array;                // release in destructor
    }
};
```

When an `IntArray` goes out of scope, its destructor automatically calls `delete[] m_array` — no manual cleanup needed at call sites.

## When destructors run

| Storage duration | Destructor called when… |
|---|---|
| Local (automatic) | Object goes out of scope |
| Static / global | Program ends normally |
| Heap-allocated | `delete` is explicitly called |

**Exception:** `std::exit()` terminates immediately and skips local destructors — a reason to avoid it in RAII-reliant code.

## Connection to memory leaks

RAII is the primary prevention for [[Memory Leaks]]: the resource-owning object cannot outlive its destructor, so there is no window where the heap block goes unfreed.

## Modern C++ RAII wrappers

Raw `new`/`delete` pairs are replaced by RAII wrappers:

| Wrapper | Manages |
|---|---|
| [[std-vector\|std::vector]] | Dynamic array |
| `std::unique_ptr` | Single heap object (unique ownership) |
| `std::shared_ptr` | Single heap object (shared ownership) |

These call `delete` (or `delete[]`) in their destructors, applying the same RAII principle automatically.

> Full coverage: [[Chapter 19 — Dynamic Allocation]] → RAII
