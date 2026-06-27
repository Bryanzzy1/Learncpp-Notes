---
tags:
  - cpp/templates
  - cpp/classes
  - concept
  - syntax
  - best-practice
aliases:
  - class template specialization
  - full class specialization
  - specializing member functions
  - Storage8<bool>
up: "[[Chapter 26 — Templates and Classes]]"
related:
  - "[[Partial Template Specialization]]"
  - "[[Partial Template Specialization for Pointers]]"
  - "[[Function Template Specialization]]"
  - "[[Class Templates]]"
  - "[[Class Template Member Functions]]"
  - "[[Template Classes]]"
  - "[[Inline Functions and Variables]]"
  - "[[Header Files]]"
  - "[[One Definition Rule]]"
  - "[[Chapter 26 — Templates and Classes]]"
---

# Class Template Specialization

**Class template specialization** lets you provide a completely different class implementation for a specific type argument, while the primary template handles everything else.

## Full class specialization

Use an empty `template<>` declaration followed by the class name with the specialized argument:

```cpp
// Primary template
template <typename T>
class Storage8
{
private:
    T m_array[8];

public:
    void set(int index, const T& value) { m_array[index] = value; }
    const T& get(int index) const       { return m_array[index]; }
};

// Full specialization for bool — completely different implementation
template <>
class Storage8<bool>
{
private:
    std::uint8_t m_data{};   // pack 8 bools into one byte

public:
    void set(int index, bool value)
    {
        auto mask{ 1 << index };
        if (value)
            m_data |= mask;
        else
            m_data &= ~mask;
    }

    bool get(int index)
    {
        auto mask{ 1 << index };
        return (m_data & mask);
    }
};
```

`Storage8<bool>` now uses bitwise packing instead of a raw array — a completely independent implementation. See [[Bit Masks]] for the bitwise techniques used here.

## Specializing individual member functions

Instead of specializing the entire class, you can specialize a single member function of the primary template:

```cpp
template <>
inline void Storage8<bool>::set(int index, bool value)
{
    // specialized implementation
}
```

The syntax mirrors [[Function Template Specialization]]. Because explicit specializations are not implicitly inline, mark them `inline` when placed in a [[Header Files|header file]] to avoid [[One Definition Rule|ODR]] violations. See [[Inline Functions and Variables]].

## Header placement

Place both the primary template and all specializations in the same header, with specializations immediately below the primary definition. Any translation unit that includes the header will see the specialization and use it automatically.

## Subnodes

- [[Partial Template Specialization]] — specialize for a *category* of types (e.g. all `T*`) rather than one specific type
- [[Partial Template Specialization for Pointers]] — the most common partial specialization pattern

> Full coverage: [[Chapter 26 — Templates and Classes]] → Class Template Specialization
