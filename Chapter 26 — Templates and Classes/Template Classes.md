---
tags:
  - cpp/templates
  - cpp/classes
  - concept
  - syntax
  - best-practice
aliases:
  - template class
  - class template ch26
up: "[[Chapter 26 — Templates and Classes]]"
related:
  - "[[Class Templates]]"
  - "[[Class Template Member Functions]]"
  - "[[Function Templates]]"
  - "[[Class Template Specialization]]"
  - "[[Template Non-type Parameters in Classes]]"
  - "[[Header Files]]"
  - "[[One Definition Rule]]"
  - "[[Chapter 26 — Templates and Classes]]"
---

# Template Classes

A **template class** is a class (or struct) definition parameterized on one or more types or values, letting the compiler generate type-specific versions on demand. The mechanics are the same as [[Function Templates]] but applied to an entire class body.

For the core syntax and usage of class templates, see [[Class Templates]] (Ch13).

## Defining member functions outside the class body

Member functions of a class template can be defined outside the class body. Each out-of-class definition needs its own `template` parameter declaration, and the class name must include the template argument:

```cpp
template <typename T>
class Array
{
public:
    T& operator[](int index);
};

template <typename T>           // required again for out-of-class definition
T& Array<T>::operator[](int index)
{
    // ...
}
```

See [[Class Template Member Functions]] for the full rules, including the injected class name shorthand.

## Header placement

Like all templates, class template definitions — including their member function definitions — must be visible in every [[Translation Unit]] that instantiates them. Put everything in [[Header Files|header files]]. The [[One Definition Rule]] permits identical template definitions across translation units.

## Relationship to specialization

Chapter 26 extends class templates with **explicit specializations** (a completely different implementation for a specific type) and **partial specializations** (a partially constrained implementation). See [[Class Template Specialization]] and [[Partial Template Specialization]].

> Full coverage: [[Chapter 26 — Templates and Classes]] → Template Classes
