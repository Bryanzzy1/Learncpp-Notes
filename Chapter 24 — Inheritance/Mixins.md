---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - cpp/templates
  - concept
  - syntax
  - subnode
aliases:
  - mixin
  - mixin class
  - CRTP
  - Curiously Recurring Template Pattern
  - curiously recurring template
up: "[[Multiple Inheritance]]"
related:
  - "[[Multiple Inheritance]]"
  - "[[Basic Inheritance]]"
  - "[[Function Templates]]"
  - "[[static_cast]]"
  - "[[Chapter 24 — Inheritance]]"
---

# Mixins

A **mixin** is a small class that can be inherited from in order to add a specific, self-contained set of properties or behaviors to another class. Mixins are not intended to be used as standalone objects — they exist purely to be mixed into derived classes.

## Basic mixin

```cpp
class Box
{
public:
    void setTopLeft(Point2D point)     { m_topLeft = point; }
    void setBottomRight(Point2D point) { m_bottomRight = point; }
private:
    Point2D m_topLeft{};
    Point2D m_bottomRight{};
};

class Button : public Box, public Label { /* ... */ };
```

`Button` gains box-sizing behavior from `Box` and label behavior from `Label` without duplicating either implementation.

## Curiously Recurring Template Pattern (CRTP)

A mixin can be parameterized on the derived class itself using a [[Function Templates|template]], giving it the ability to call back into the derived class without virtual dispatch:

```cpp
template <class T>
class Mixin
{
    // Can access Derived members via static_cast<T*>(this)
    // (see [[static_cast]])
};

class Derived : public Mixin<Derived>
{
};
```

This is the **Curiously Recurring Template Pattern (CRTP)**. Because the base template is instantiated with the concrete derived type, the mixin can perform compile-time polymorphism — calling derived methods or accessing derived members through a `static_cast`, without the overhead of virtual function dispatch.

CRTP is a common pattern in policy-based design and zero-overhead abstractions in C++ libraries.

> Full coverage: [[Chapter 24 — Inheritance]] → Multiple Inheritance → Mixins
