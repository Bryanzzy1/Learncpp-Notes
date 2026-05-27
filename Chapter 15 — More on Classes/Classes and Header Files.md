---
tags:
  - cpp/classes
  - cpp/files
  - concept
  - syntax
  - best-practice
aliases:
  - class declaration
  - class header
  - class in header
  - member function definition outside class
up: "[[Chapter 15 — More on Classes]]"
related:
  - "[[Classes]]"
  - "[[Member Functions]]"
  - "[[Header Files]]"
  - "[[Inline Functions and Variables]]"
  - "[[One Definition Rule]]"
  - "[[Default Arguments]]"
  - "[[Translation Unit]]"
---

# Classes and Header Files

## Defining member functions outside the class

Member functions can be defined outside the class definition, just like non-member functions. The function name must be prefixed with the class name and `::` so the compiler knows it is a member:

```cpp
// Date.h
class Date
{
public:
    void print() const; // declaration only
};

// Date.cpp
void Date::print() const   // Date:: prefix required
{
    std::cout << m_year << '/' << m_month << '/' << m_day;
}
```

## Standard layout: declaration in .h, definition in .cpp

The recommended pattern mirrors non-member functions:

| File | Contains |
|------|---------|
| `ClassName.h` | Class definition + member function declarations |
| `ClassName.cpp` | Member function definitions |

This keeps the [[One Definition Rule]] satisfied: the class definition (header) can be `#include`d in many [[Translation Unit|translation units]], while the function bodies appear in exactly one `.cpp`.

## Inline member functions in headers

Member functions defined **inside** the class body are **implicitly `inline`**, which allows them to be `#include`d into multiple `.cpp` files without violating the [[One Definition Rule]].

Member functions defined **outside** the class body can also remain in the header if explicitly marked `inline`:

```cpp
// Still in Date.h, outside the class body
inline void Date::print() const
{
    std::cout << m_year << '/' << m_month << '/' << m_day;
}
```

See [[Inline Functions and Variables]] for the full rules on inline.

## Default arguments

Put default argument values for member functions inside the **class definition** (the header), not in the out-of-class definition. The compiler must see default arguments at the call site, which means they must be visible when the header is included.

See [[Default Arguments]] for the general rule.

> Full coverage: [[Chapter 15 — More on Classes]] → Classes and Header Files
