---
tags:
  - cpp/classes
  - cpp/constructors
  - syntax
  - subnode
aliases:
  - member initializer list
  - member initialization list
  - constructor initializer list
up: "[[Constructors]]"
related:
  - "[[Constructors]]"
  - "[[Default Constructor]]"
  - "[[Delegating Constructors]]"
  - "[[Initialization]]"
  - "[[Classes]]"
---

# Member Initializer List

The **member initializer list** is the preferred mechanism for initializing data members inside a constructor. It appears between the `:` after the parameter list and the opening `{` of the constructor body.

```cpp
Foo(int x, int y)
    : m_x { x }, m_y { y }
{
    // constructor body — usually empty
}
```

## Rules

**Order**: list members in the same order they are **declared in the class**. Data members are initialized in declaration order regardless of the order they appear in the initializer list — mismatched order can cause hard-to-spot bugs.

**Priority**: if a member has both a **default member initializer** (defined at the class level) and appears in the **member initializer list**, the member initializer list value **takes precedence**.

**Constructor body**: the body of a constructor is executed after all member initializations are complete. Constructor bodies are most often left empty — initialization belongs in the initializer list, not the body.

## Why prefer it over assignment in the body

Using the body to assign values:

```cpp
Foo(int x, int y)
{
    m_x = x; // This is assignment, not initialization
    m_y = y;
}
```

This first default-initializes the members, then assigns to them — two steps instead of one. For `const` members or reference members, assignment is impossible; they **must** be initialized in the member initializer list.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Member Initializer List
