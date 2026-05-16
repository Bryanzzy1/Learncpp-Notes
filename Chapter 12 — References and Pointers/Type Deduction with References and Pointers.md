---
tags:
  - cpp/type-deduction
  - cpp/references
  - cpp/pointers
  - cpp/const
  - concept
  - syntax
aliases:
  - type deduction references
  - type deduction pointers
  - top-level const
  - low-level const
up: "[[Chapter 12 — References and Pointers]]"
related:
  - "[[Pointers]]"
  - "[[Lvalue References]]"
  - "[[Lvalue References to Const]]"
  - "[[Type Deduction]]"
  - "[[constexpr]]"
---

# Type Deduction with References and Pointers

When [[Type Deduction|`auto`]] is used with references or const-qualified types, extra rules apply beyond ordinary const-dropping.

## References are dropped

Type deduction drops references: if the initializer is a reference, `auto` deduces the **referenced type**, not the reference type.

```cpp
int x { 5 };
int& ref { x };
auto a { ref }; // deduces int, not int&
```

## Top-level const vs. low-level const

| Term | Meaning | Example |
| --- | --- | --- |
| **Top-level const** | const applies to the object itself | `const int` (the int is const) |
| **Low-level const** | const applies to what is referenced/pointed to | `const int&`, `const int*` |

Type deduction **drops top-level const** from the deduced type, but **preserves low-level const**.

## Type deduction and const references

Dropping a reference can convert a low-level const into a top-level const. `const std::string&` is a low-level const; after dropping the reference you get `const std::string`, a top-level const that `auto` then also drops:

```cpp
const std::string s { "hello" };
const std::string& ref { s };
auto a { ref }; // deduces std::string (reference dropped, then top-level const dropped)
```

**Best practice:** reapply `const` and `&` explicitly when you want a const reference — it makes intent clear and prevents mistakes:

```cpp
const auto& a { ref }; // explicitly a const reference to std::string
```

## Type deduction and pointers

Unlike references, **type deduction does not drop pointers**. If the initializer is a pointer, `auto` preserves the pointer type (though top-level const on the pointer itself is still dropped).

> Full coverage: [[Chapter 12 — References and Pointers]] → Type Deduction with References and Pointers
