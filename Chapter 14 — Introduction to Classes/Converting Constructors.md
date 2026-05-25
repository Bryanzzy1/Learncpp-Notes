---
tags:
  - cpp/classes
  - cpp/constructors
  - concept
  - syntax
  - best-practice
aliases:
  - converting constructor
  - explicit constructor
  - explicit keyword
  - implicit conversion constructor
up: "[[Chapter 14 — Introduction to Classes]]"
related:
  - "[[Constructors]]"
  - "[[Default Constructor]]"
  - "[[Copy Constructor]]"
  - "[[Implicit Type Conversion]]"
  - "[[Narrowing Conversions]]"
  - "[[static_cast]]"
  - "[[Initialization]]"
---

# Converting Constructors

A **converting constructor** is any constructor that the compiler can use to perform an **implicit conversion** from an argument type to the class type. By default, **all constructors are converting constructors**.

## One user-defined conversion at a time

The compiler applies **at most one** user-defined conversion in an implicit conversion sequence. Two consecutive user-defined conversions are not allowed.

## The `explicit` keyword

Mark a constructor `explicit` to prevent the compiler from using it for implicit conversions:

```cpp
class Dollars
{
public:
    explicit Dollars(int d)
        : m_dollars{ d }
    {
    }
private:
    int m_dollars;
};
```

An `explicit` constructor **cannot** be used for:
- Copy initialization (`Dollars d = 5;` — fails)
- Copy list initialization (`Dollars d = { 5 };` — fails)
- Implicit conversions (e.g. passing `5` where `Dollars` is expected — fails)

It **can** still be used for direct initialization and direct list initialization:
```cpp
Dollars d1 { 5 };   // OK: direct list initialization
Dollars d2(5);      // OK: direct initialization
```

## Return value implicit conversion

When a function returns by value and the returned expression doesn't match the return type, an implicit conversion occurs — subject to the same `explicit` restrictions as pass by value. An `explicit` constructor cannot be used here.

## When to use `explicit`

| Constructor type | Should be `explicit`? |
|-------------------|----------------------|
| Single-argument constructors | **Yes** — prevents surprising implicit conversions |
| Copy/move constructors | **No** — these copy, not convert |
| Default (no-parameter) constructors | Typically **no** |
| Multi-argument constructors | Typically **no** — unlikely to be used for conversion anyway |

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Converting Constructors
