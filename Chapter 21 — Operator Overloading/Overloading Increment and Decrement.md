---
tags:
  - cpp/operators
  - cpp/classes
  - concept
  - syntax
aliases:
  - overload operator++
  - overload operator--
  - prefix increment overload
  - postfix increment overload
  - prefix decrement overload
  - postfix decrement overload
up: "[[Chapter 21 — Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Member Function Overloads]]"
  - "[[Increment and Decrement]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Overloading Increment and Decrement

`operator++` and `operator--` each have two forms — **prefix** and **postfix** — that must be differentiated at the declaration level.

> For the built-in behavior of `++`/`--` on fundamental types, see [[Increment and Decrement]].

## Differentiating prefix from postfix

The compiler uses a dummy `int` parameter to distinguish postfix from prefix:

```cpp
Digit& operator++();     // prefix: no parameter
Digit& operator--();     // prefix: no parameter

Digit operator++(int);   // postfix: dummy int parameter
Digit operator--(int);   // postfix: dummy int parameter
```

The dummy `int` is never used — it exists only so the signatures differ.

## Implementing prefix

Prefix modifies the object and returns a reference to it (the modified value):

```cpp
Digit& Digit::operator++()
{
    if (m_digit == 9)
        m_digit = 0;   // wrap around
    else
        ++m_digit;
    return *this;
}
```

## Implementing postfix

Postfix must return the **original** value (before modification), so it saves a copy first:

```cpp
Digit Digit::operator++(int) // dummy int signals postfix
{
    Digit temp{ *this }; // save current state
    ++(*this);           // use prefix operator to do the actual increment
    return temp;         // return original value
}
```

Postfix returns **by value** (a copy of the old state), not by reference.

## Key point

Both prefix and postfix perform the same underlying increment/decrement. The difference is only in what value they return — prefix returns the new value, postfix returns the old value.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Overloading Increment and Decrement
