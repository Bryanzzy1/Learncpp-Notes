---
tags:
  - cpp/classes
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - const member function
  - const object
  - const method
up: "[[Classes]]"
related:
  - "[[Classes]]"
  - "[[Member Functions]]"
  - "[[Member Access]]"
  - "[[Lvalue References to Const]]"
  - "[[constexpr]]"
  - "[[Constexpr Classes]]"
---

# Const Member Functions

A **const member function** is a member function that promises not to modify the implicit object. It is declared by appending `const` after the parameter list:

```cpp
void print() const
{
    std::cout << m_year << '/' << m_month << '/' << m_day;
}
```

## Why this matters: const objects

A `const` class object cannot have its data members modified. To enforce this, **const objects may only call `const` member functions**:

```cpp
const Date d { 2024, 1, 1 };
d.print();   // OK: print() is const
d.setYear(2025); // compile error: setYear() is non-const
```

## What a const member function can and cannot do

| Allowed | Not allowed |
|---------|------------|
| Read data members | Modify the implicit object's data members |
| Call other `const` member functions | Call non-`const` member functions |
| Call non-member functions | |
| Modify objects that are not the implicit object | |

## Best practice

A member function that **does not and will never modify the object's state** should be marked `const`. This allows it to be called on both `const` and non-`const` objects, making the class more flexible.

This interacts with [[Lvalue References to Const]] — when a class object is passed by `const` reference, only `const` member functions are accessible on it.

For `constexpr` member functions, note that as of C++14, `constexpr` does not imply `const` — you must write both qualifiers if you need both (see [[Constexpr Classes]]).

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Const Member Functions
