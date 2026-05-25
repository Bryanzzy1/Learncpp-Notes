---
tags:
  - cpp/classes
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - access specifier
  - public member
  - private member
  - protected member
  - access function
  - getter
  - setter
  - accessor
  - mutator
  - m_ prefix
up: "[[Classes]]"
related:
  - "[[Classes]]"
  - "[[Member Functions]]"
  - "[[Data Hiding]]"
  - "[[Structs]]"
  - "[[Lvalue References to Const]]"
  - "[[Pass by Reference]]"
---

# Member Access

C++ class types control which members are accessible from outside the class using **access specifiers**.

## Access levels

| Specifier | Who can access |
|-----------|---------------|
| `public` | Anyone — no restrictions |
| `private` | Only other members of the same class |
| `protected` | Members of the same class **and** derived classes (see inheritance) |

- **`struct` members are `public` by default**
- **`class` members are `private` by default**

This is the only formal difference between `struct` and `class`.

## Access specifier syntax

```cpp
class Date
{
public:
    void print() const
    {
        std::cout << m_year << '/' << m_month << '/' << m_day;
    }

private:
    int m_year { 2020 };
    int m_month { 14 };
    int m_day { 10 };
};
```

Members following a specifier inherit that access level until the next specifier.

## Per-class, not per-object

Access is enforced **per class type**, not per individual object. A member function can directly access the private members of **any** object of the same class type that is in scope — not just `this`.

## Private data members: naming convention

By convention, private data members are named with an `m_` prefix (e.g. `m_year`, `m_month`) to distinguish them from local variables and parameters.

A class with private members is **no longer an aggregate** — aggregate initialization no longer works.

## Access functions (getters and setters)

An **access function** is a trivial public member function that reads or writes a private data member.

- **Getter (accessor)**: returns the value of a private member
  - Should return **by value** or **by `const` lvalue reference** (see [[Lvalue References to Const]])
  - Do **not** return a non-`const` reference to a private member — this defeats encapsulation
- **Setter (mutator)**: modifies the value of a private member

Data members have the **same lifetime as the object** containing them. Prefer to use the return value of a getter that returns by reference **immediately** — storing a reference to a member of a temporary (rvalue) object creates a dangling reference.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Member Access
