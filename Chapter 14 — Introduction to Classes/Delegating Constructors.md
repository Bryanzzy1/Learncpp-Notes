---
tags:
  - cpp/classes
  - cpp/constructors
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - delegating constructor
  - constructor delegation
up: "[[Constructors]]"
related:
  - "[[Constructors]]"
  - "[[Member Initializer List]]"
  - "[[Default Constructor]]"
  - "[[Temporary Class Objects]]"
  - "[[List Initialization]]"
---

# Delegating Constructors

**Delegating constructors** allow one constructor to call another constructor of the same class, avoiding duplicate initialization code across overloads.

Delegation is expressed in the **member initializer list** — the delegating constructor calls the target constructor there:

```cpp
class Foo
{
public:
    Foo(int x, int y)
        : m_x { x }, m_y { y }
    {
    }

    Foo()
        : Foo(0, 0) // delegates to Foo(int, int)
    {
    }

private:
    int m_x{};
    int m_y{};
};
```

## Key rules

**Do not call a constructor directly from the body of another function.** Doing so either causes a compilation error or direct-initializes a **temporary object** (which is immediately discarded) rather than the current object:

```cpp
Foo()
{
    Foo(0, 0); // Wrong: creates and discards a temporary, does NOT initialize *this
}
```

If you intentionally want a temporary object, use [[List Initialization|list initialization]] to make the intent clear:

```cpp
Foo temp { 0, 0 }; // clear: creating a named temporary
```

## When to use

If you have multiple constructors with overlapping initialization logic, consider delegating constructors to centralize that logic. This reduces duplication and keeps initialization consistent.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Delegating Constructors
