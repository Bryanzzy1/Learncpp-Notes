---
tags:
  - cpp/classes
  - cpp/constexpr
  - cpp/optimization
  - concept
  - syntax
aliases:
  - constexpr class
  - literal type
  - constexpr member function
  - constexpr constructor
up: "[[Chapter 14 — Introduction to Classes]]"
related:
  - "[[Classes]]"
  - "[[Structs]]"
  - "[[constexpr]]"
  - "[[constexpr Functions]]"
  - "[[Compile-time Evaluation]]"
  - "[[Const Member Functions]]"
---

# Constexpr Classes

## Literal types

A **literal type** is any type for which it is possible to create an object within a constant expression. Aggregates (like [[Structs|structs]] without constructors) **implicitly support `constexpr`** — no special work is needed.

## Making a class constexpr-capable

To allow a `class` (with constructors and member functions) to be used in constant expressions, mark both the constructor and the relevant member functions `constexpr`:

```cpp
class Point
{
public:
    constexpr Point(int x, int y) : m_x{ x }, m_y{ y } {}
    constexpr int getX() const { return m_x; }
    constexpr int getY() const { return m_y; }
private:
    int m_x{};
    int m_y{};
};

constexpr Point origin { 0, 0 }; // evaluated at compile time
```

When a `constexpr` function is evaluated in a **compile-time context**, only other `constexpr` functions may be called (see [[Compile-time Evaluation]]).

## `constexpr` vs `const` on member functions

As of **C++14**, `constexpr` member functions are **no longer implicitly `const`**. If you want a `constexpr` member function that is also `const`, you must write both qualifiers:

```cpp
constexpr const int& getX() const { return m_x; }
//  ^-- evaluatable at compile time
//                   ^-- return type is const reference
//                            ^-- function is const (safe on const objects)
```

A `constexpr` non-const member function **can modify data members** — this is legal as long as the object itself is not `const`.

Compare with [[constexpr Functions]] for the rules governing `constexpr` free functions.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Constexpr Classes
