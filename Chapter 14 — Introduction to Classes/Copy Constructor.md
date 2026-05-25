---
tags:
  - cpp/classes
  - cpp/constructors
  - concept
  - syntax
  - best-practice
aliases:
  - copy constructor
  - implicit copy constructor
  - memberwise copy
  - rule of three
  - rule of five
  - = delete constructor
up: "[[Chapter 14 — Introduction to Classes]]"
related:
  - "[[Copy Elision]]"
  - "[[Constructors]]"
  - "[[Initialization]]"
  - "[[Pass by Value]]"
  - "[[Pass by Reference]]"
  - "[[Lvalue References to Const]]"
  - "[[Member Initializer List]]"
---

# Copy Constructor

A **copy constructor** is a constructor used to initialize an object with an existing object of the **same type**. After it executes, the new object should be an exact copy of the source.

## Implicit copy constructor

If you don't define a copy constructor, C++ generates a `public` **implicit copy constructor** that performs **memberwise initialization** — each member is initialized from the corresponding member of the source object.

## Writing your own

```cpp
Fraction(const Fraction& fraction)
    : m_numerator{ fraction.m_numerator }
    , m_denominator{ fraction.m_denominator }
{
}
```

Rules:
- The parameter **must be a `const` lvalue reference** (see [[Lvalue References to Const]]). Without `const`, passing a const object would not compile. Without a reference, copying would require calling the copy constructor — infinite recursion.
- Copy constructors should have **no side effects** beyond copying.
- **Prefer the implicit copy constructor** unless you have a specific reason to write your own (e.g. deep copying a pointer).

## Controlling copyability

```cpp
Fraction(const Fraction& fraction) = default; // explicit request for default behavior
Fraction(const Fraction& fraction) = delete;  // disable copying entirely
```

Using `= delete` makes any attempt to copy the object a compile error.

## Pass by value invokes the copy constructor

When an object is [[Pass by Value|passed by value]], the copy constructor is called implicitly to initialize the parameter from the argument.

## Rule of three / rule of five

**Rule of three**: if a class needs a user-defined copy constructor, destructor, or copy assignment operator, it probably needs all three. This arises when a class manages a resource (e.g. a raw pointer).

**Rule of five** (C++11): extends the rule of three to also include the **move constructor** and **move assignment operator**.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Copy Constructor
