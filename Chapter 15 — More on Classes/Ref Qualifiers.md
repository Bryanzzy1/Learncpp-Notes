---
tags:
  - cpp/classes
  - cpp/functions
  - concept
  - syntax
aliases:
  - ref qualifier
  - lvalue ref qualifier
  - rvalue ref qualifier
  - "&& member function"
  - "& member function"
up: "[[Chapter 15 — More on Classes]]"
related:
  - "[[this Pointer]]"
  - "[[Const Member Functions]]"
  - "[[Member Functions]]"
  - "[[Value Categories]]"
  - "[[Lvalue References]]"
  - "[[Lvalue References to Const]]"
  - "[[Member Access]]"
---

# Ref Qualifiers

**Ref qualifiers** (introduced in C++11) allow overloading a member function based on whether the implicit object is an **lvalue** or an **rvalue**.

## Syntax

Append `&` or `&&` after the cv-qualifiers (`const`/`volatile`) in the function signature:

```cpp
class Employee
{
    std::string m_name{};

public:
    // Called when the implicit object is a (possibly const) lvalue
    const std::string& getName() const &  { return m_name; }

    // Called when the implicit object is a non-const rvalue
    std::string        getName() const && { return m_name; }
};
```

The `&`-qualified overload returns by reference (cheap); the `&&`-qualified overload returns by value (a copy is expected anyway for temporaries).

## Using `std::move` in the rvalue overload

For non-const rvalue objects, moving from the member avoids an unnecessary copy:

```cpp
std::string getName() && { return std::move(m_name); }
```

## Rules

| Rule | Detail |
|------|--------|
| Cannot mix | Non-ref-qualified and ref-qualified overloads of the same function **cannot coexist** |
| `const &` fallback | If only a `const &`-qualified overload exists, it accepts both lvalue and rvalue implicit objects |
| `= delete` | Either qualified overload can be explicitly deleted to prevent that call |

## Recommendation

The course does **not** recommend ref qualifiers as a general best practice. The simpler alternative: **always use the return value of an access function immediately** and never store returned references for later use. This avoids the dangling reference problem that ref qualifiers are designed to work around.

See [[Member Access]] for the general guidance on getters returning references.

> Full coverage: [[Chapter 15 — More on Classes]] → Ref Qualifiers
