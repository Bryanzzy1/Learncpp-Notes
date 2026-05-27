---
tags:
  - cpp/classes
  - cpp/static
  - concept
  - syntax
  - best-practice
aliases:
  - static member variable
  - static data member
  - class-level variable
  - inline static
up: "[[Chapter 15 — More on Classes]]"
related:
  - "[[Static Member Functions]]"
  - "[[Classes]]"
  - "[[Global Variables]]"
  - "[[Static Local Variables]]"
  - "[[Inline Functions and Variables]]"
  - "[[Member Access]]"
  - "[[Header Files]]"
  - "[[One Definition Rule]]"
---

# Static Member Variables

A **static member variable** is a member variable shared by **all objects** of the class — there is only one copy regardless of how many instances exist. It lives in static storage (like a [[Global Variables|global variable]]), but is scoped to the class.

```cpp
class Something
{
public:
    static int s_value; // declaration only
};

int Something::s_value { 1 }; // definition (outside class)
```

## Access

Because a static member exists independently of any object, it can be accessed through the class name directly:

```cpp
Something::s_value = 3;       // preferred: use class name
Something s{};
s.s_value = 3;                // works but misleading — avoid
```

## Definition and initialization

Static members must be **defined exactly once**, outside the class definition (usually in the corresponding `.cpp`):

```cpp
int Something::s_value { 1 };
```

This definition is not subject to access controls — you can define a `private` static member from outside the class.

### Inline static members (C++17)

Prefer `inline` static members, which can be defined and initialized **inside** the class body regardless of whether they are `const`:

```cpp
class Whatever
{
public:
    static inline int s_count { 0 };           // non-const inline static
    static const int s_limit { 100 };          // const static (old style, allowed for integral types)
};
```

Inline statics satisfy the [[One Definition Rule]] when the header is included in multiple translation units.

## Type deduction

Only static members may use type deduction (`auto`) or [[CTAD]] — non-static members cannot.

## Subnodes

- [[Static Member Functions]] — functions that operate on static members and have no `this` pointer

> Full coverage: [[Chapter 15 — More on Classes]] → Static Member Variables
