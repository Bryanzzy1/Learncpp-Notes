---
tags:
  - cpp/operators
  - cpp/classes
  - concept
  - syntax
  - best-practice
aliases:
  - copy assignment operator
  - operator= overload
  - assignment operator overload
  - self-assignment
  - implicit copy assignment
up: "[[Chapter 21 — Operator Overloading]]"
related:
  - "[[Shallow vs Deep Copying]]"
  - "[[Copy Constructor]]"
  - "[[Member Function Overloads]]"
  - "[[new and delete]]"
  - "[[Memory Leaks]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Overloading the Assignment Operator

The **copy assignment operator** (`operator=`) defines what happens when you assign an existing object from another existing object of the same type. It must be overloaded as a **member function**.

## Copy constructor vs copy assignment

| | [[Copy Constructor]] | Copy assignment operator |
|---|---|---|
| When invoked | Initializing a **new** object | Assigning to an **already-existing** object |
| Implicit version | Yes (memberwise init) | Yes (memberwise copy) |

## Basic implementation

```cpp
Fraction& Fraction::operator=(const Fraction& fraction)
{
    m_numerator   = fraction.m_numerator;
    m_denominator = fraction.m_denominator;
    return *this; // return *this to allow chaining: a = b = c
}
```

Returning `*this` by reference is the convention — it enables chained assignment.

## Self-assignment

When a class manages dynamic memory (see [[new and delete]]), self-assignment (`obj = obj`) is dangerous: you may free the memory you're about to read from. Always add a self-assignment guard:

```cpp
Fraction& Fraction::operator=(const Fraction& fraction)
{
    if (this == &fraction)  // self-assignment check
        return *this;

    // ... copy members ...
    return *this;
}
```

The self-assignment check is typically **skipped in copy constructors** — a newly constructed object cannot be the same object as the source.

## Implicit copy assignment operator

The compiler provides a `public` implicit copy assignment operator (memberwise copy) if you don't define one. This is fine for classes that don't manage resources. For classes that own heap memory, define your own — and follow the **rule of three**: if you need a custom copy constructor, destructor, or copy assignment operator, you probably need all three (see [[Copy Constructor]]).

## Deep copying

When the class contains a pointer to heap-allocated memory, memberwise copy copies the **pointer**, not the data — both objects then point to the same memory ([[Shallow vs Deep Copying]]). A proper copy assignment operator must allocate new memory and copy the pointed-to data.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Overloading the Assignment Operator
