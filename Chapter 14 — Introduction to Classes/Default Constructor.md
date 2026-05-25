---
tags:
  - cpp/classes
  - cpp/constructors
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - default constructor
  - implicit default constructor
  - explicitly defaulted constructor
  - = default
  - overloaded constructor
up: "[[Constructors]]"
related:
  - "[[Constructors]]"
  - "[[Member Initializer List]]"
  - "[[Delegating Constructors]]"
  - "[[Initialization]]"
  - "[[List Initialization]]"
  - "[[Structs]]"
---

# Default Constructor

A **default constructor** is a constructor that accepts no arguments. It allows objects to be created without providing initialization values.

```cpp
class Foo
{
public:
    Foo() // default constructor
        : m_x { 0 }, m_y { 0 }
    {
    }
private:
    int m_x{};
    int m_y{};
};

Foo f {}; // calls default constructor
```

A constructor with all default arguments also acts as a default constructor:

```cpp
Foo(int x = 0, int y = 0)
    : m_x { x }, m_y { y }
{
}
```

## Implicit default constructor

If a non-aggregate class type has **no user-declared constructors**, the compiler generates a `public` **implicit default constructor**. This allows the class to be value- or default-initialized.

## Using `= default`

You can explicitly request the compiler to generate a default constructor:

```cpp
Foo() = default;
```

Prefer `= default` over an empty constructor body `Foo() {}`. They are nearly identical, but have one important difference in value initialization:

| Constructor type | Value initialization behavior |
|-----------------|-------------------------------|
| User-provided (even empty body) | Object is **default initialized** |
| Not user-provided (`= default` or implicit) | Object is **zero-initialized**, then default initialized |

This means `= default` gives you zero-initialization of members before the constructor runs, while `Foo() {}` does not.

## Aggregates (pre-C++20)

Prior to C++20, a class with a **user-provided** default constructor (even one with an empty body) is **no longer an aggregate** — aggregate initialization is disabled. An explicitly defaulted constructor (`= default`) does not make the class a non-aggregate.

## Best practice

Prefer **value initialization** (`Foo f {}`) over default initialization (`Foo f`) for class types.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Default Constructor
