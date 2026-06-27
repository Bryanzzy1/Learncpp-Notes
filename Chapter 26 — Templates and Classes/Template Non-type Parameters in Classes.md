---
tags:
  - cpp/templates
  - cpp/classes
  - concept
  - syntax
aliases:
  - non-type parameter in class template
  - NTTP in class
  - compile-time size
  - template value in class
up: "[[Chapter 26 — Templates and Classes]]"
related:
  - "[[Non-type Template Parameters]]"
  - "[[Class Templates]]"
  - "[[Class Template Specialization]]"
  - "[[Template Classes]]"
  - "[[constexpr]]"
  - "[[Chapter 26 — Templates and Classes]]"
---

# Template Non-type Parameters in Classes

A **non-type template parameter** (NTTP) in a class template is a compile-time constant value (not a type) that becomes part of the class's definition. The most common use is baking an array size directly into the class type.

For NTTPs in function templates, see [[Non-type Template Parameters]] (Ch11).

## Syntax

```cpp
template <typename T, int size>   // size is an integral non-type parameter
class StaticArray
{
private:
    T m_array[size] {};           // size is a compile-time constant here

public:
    T* getArray();

    T& operator[](int index)
    {
        return m_array[index];
    }
};

StaticArray<int, 12> array{};     // T = int, size = 12
```

`StaticArray<int, 12>` and `StaticArray<int, 24>` are **different types** — the NTTP is baked into the type identity.

## Allowed non-type parameter types

| Category | Examples |
|---|---|
| Integral types | `int`, `std::size_t`, `char`, `bool` |
| Enumeration types | any `enum` or `enum class` |
| Pointer/reference to class object | `const MyClass*` |
| Pointer/reference to function | `void(*)(int)` |
| Pointer/reference to member function | `void(MyClass::*)()` |
| `std::nullptr_t` | `nullptr` |
| Floating-point types (C++20) | `float`, `double` |

## Constexpr requirement

All template arguments for non-type parameters must be [[constexpr|constant expressions]]:

```cpp
int n{ 12 };
StaticArray<int, n> arr{};       // ✗ — n is not constexpr

constexpr int m{ 12 };
StaticArray<int, m> arr2{};      // ✓ — m is constexpr
```

> Full coverage: [[Chapter 26 — Templates and Classes]] → Template Non-type Parameters in Classes
