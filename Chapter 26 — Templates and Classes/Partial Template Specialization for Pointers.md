---
tags:
  - cpp/templates
  - cpp/classes
  - concept
  - syntax
  - subnode
aliases:
  - partial specialization for pointers
  - pointer partial specialization
  - T* specialization
up: "[[Partial Template Specialization]]"
related:
  - "[[Partial Template Specialization]]"
  - "[[Class Template Specialization]]"
  - "[[Class Templates]]"
  - "[[Chapter 26 — Templates and Classes]]"
---

# Partial Template Specialization for Pointers

Partially specializing a class template for `T*` provides a separate implementation for **any pointer type**, while the primary template handles non-pointer types. This is one of the most common and idiomatic uses of partial specialization.

## Pattern

```cpp
// Primary template — handles non-pointer types
template <typename T>
class Storage
{
private:
    T m_value{};

public:
    void set(const T& value) { m_value = value; }
    const T& get() const     { return m_value; }
};

// Partial specialization — handles T* (any pointer type)
template <typename T>
class Storage<T*>
{
private:
    T* m_value{};

public:
    void set(T* value) { m_value = value; }
    T*  get() const    { return m_value; }
};
```

`Storage<int*>` now uses the `T*` specialization with `T = int`, while `Storage<int>` uses the primary template.

## Why this matters

Pointer types often need different handling:
- Ownership semantics (does the class own the pointed-to object?)
- Deep vs shallow copy behavior
- Null-pointer checks

Without a pointer specialization, the primary template would treat `T*` like any other value type, which is usually incorrect.

## Relationship to full specialization

| | Full specialization | Partial specialization for pointers |
|---|---|---|
| `template<>` header | Empty | Non-empty (`template <typename T>`) |
| Matches | Exactly one type | All `T*` for any `T` |
| Scope | One concrete instantiation | Entire family of pointer types |

> Full coverage: [[Chapter 26 — Templates and Classes]] → Class Template Specialization → Partial Template Specialization → Partial Template Specialization for Pointers
