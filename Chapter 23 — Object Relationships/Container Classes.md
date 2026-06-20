---
tags:
  - cpp/classes
  - cpp/containers
  - concept
  - best-practice
aliases:
  - container class
  - value container
  - reference container
up: "[[Chapter 23 — Object Relationships]]"
related:
  - "[[Object Composition]]"
  - "[[Composition]]"
  - "[[Aggregation]]"
  - "[[std-initializer-list|std::initializer_list]]"
  - "[[Classes]]"
  - "[[new and delete]]"
  - "[[Chapter 23 — Object Relationships]]"
---

# Container Classes

A **container class** is a class designed to hold and organize multiple instances of another type (a class or a fundamental type). Standard library examples include `std::vector`, `std::array`, and `std::string`.

## Two kinds of containers

| Kind | Relationship to elements | Manages element lifetime? | Implementation |
|---|---|---|---|
| **Value container** | [[Composition]] — owns copies of elements | Yes | Stores elements by value; creates/destroys them |
| **Reference container** | [[Aggregation]] — stores pointers or references to external elements | No | Stores pointers/references; elements live elsewhere |

### Value container example

```cpp
class IntArray
{
    int* m_data{};
    int  m_size{};
public:
    IntArray(int size) : m_data{ new int[size]{} }, m_size{ size } {}
    ~IntArray() { delete[] m_data; }
};
```

The array owns its data — it allocates and deallocates via [[new and delete]].

### Reference container example

A container storing `std::reference_wrapper<T>` elements holds references to externally managed objects — it does not own them and is not responsible for their destruction.

## Initializer-list support

Container classes typically support [[std-initializer-list|std::initializer_list]] constructors so they can be initialized with brace-enclosed element lists:

```cpp
IntArray arr{ 5, 4, 3, 2, 1 };
```

If you provide a list constructor, also provide a list assignment operator for consistency.

> Full coverage: [[Chapter 23 — Object Relationships]] → Container Classes
