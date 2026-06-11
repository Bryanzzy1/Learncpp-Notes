---
tags:
  - cpp/operators
  - cpp/classes
  - concept
  - syntax
  - subnode
aliases:
  - friend operator overload
  - overloading with friend functions
up: "[[Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Normal Function Overloads]]"
  - "[[Member Function Overloads]]"
  - "[[Operator Overload Selection Guide]]"
  - "[[Friend Functions]]"
  - "[[Data Hiding]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Friend Function Overloads

Overloading an operator as a **friend function** means the operator is a non-member function that has been granted access to the class's `private` and `protected` members via a `friend` declaration.

```cpp
class Cents
{
private:
    int m_cents{};

public:
    Cents(int cents) : m_cents{ cents } {}

    // Declare operator+ as a friend so it can access m_cents
    friend Cents operator+(const Cents& c1, const Cents& c2);
};

Cents operator+(const Cents& c1, const Cents& c2)
{
    return Cents{ c1.m_cents + c2.m_cents }; // direct private access
}
```

## Defining a friend inside the class body

Friend functions can be defined inline within the class — they are still non-members:

```cpp
class Cents
{
    friend Cents operator+(const Cents& c1, const Cents& c2)
    {
        return Cents{ c1.m_cents + c2.m_cents };
    }
};
```

## When to use

Friend function overloads are appropriate when the operator needs direct access to private members and cannot be implemented cleanly through the public interface. See [[Friend Functions]] for the general mechanics of friendship.

Prefer [[Normal Function Overloads]] over friends when the public interface is sufficient — friends couple the operator to the class's private implementation, weakening [[Data Hiding|encapsulation]].

See [[Operator Overload Selection Guide]] for the complete decision matrix.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Operator Overloading → Friend Function Overloads
