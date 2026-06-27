---
tags:
  - cpp/templates
  - cpp/functions
  - concept
  - syntax
  - best-practice
aliases:
  - function template specialization
  - explicit template specialization
  - full specialization
  - template<>
up: "[[Chapter 26 — Templates and Classes]]"
related:
  - "[[Function Templates]]"
  - "[[Class Template Specialization]]"
  - "[[Partial Template Specialization]]"
  - "[[Inline Functions and Variables]]"
  - "[[Header Files]]"
  - "[[One Definition Rule]]"
  - "[[Chapter 26 — Templates and Classes]]"
---

# Function Template Specialization

**Explicit template specialization** lets you define a completely different implementation of a template for a specific type (or set of types), while keeping the general template for everything else.

## Full specialization

A **full specialization** provides a concrete implementation with all template parameters fixed. The syntax uses an empty `template<>` declaration:

```cpp
// Primary template
template <typename T>
void print(const T& t)
{
    std::cout << t << '\n';
}

// Full specialization for double
template <>
void print<double>(const double& d)
{
    std::cout << std::scientific << d << '\n';
}
```

When `print<double>(3.14)` is called, the compiler uses the specialization instead of the primary template.

## Full vs partial specialization

| | Full specialization | Partial specialization |
|---|---|---|
| Parameters remaining | None (`template<>`) | Some still generic |
| Applies to | Functions and classes | **Classes only** (not functions) |

You **cannot** write a partial specialization for a function template — only for class templates. See [[Partial Template Specialization]].

## Inline requirement

Full function specializations are **not** implicitly inline (unlike the primary template). If you place a specialization in a header file that is included by multiple translation units, mark it `inline` to avoid an [[One Definition Rule|ODR]] violation:

```cpp
template <>
inline void print<double>(const double& d)
{
    std::cout << std::scientific << d << '\n';
}
```

See [[Inline Functions and Variables]] for why `inline` suppresses the ODR restriction.

## Header placement

Define the primary template and all its specializations in the same [[Header Files|header file]], with specializations immediately below the primary. This ensures any translation unit that uses the primary also sees the specializations, preventing the compiler from silently instantiating the wrong version.

## Relationship to function overloading

Prefer **overloading** over function template specialization when possible — overloads participate in overload resolution more predictably. Template specializations are invisible during overload resolution; the compiler picks the primary template first, then picks the best matching specialization among those for that template. This can produce surprising behavior when combined with overloads.

> Full coverage: [[Chapter 26 — Templates and Classes]] → Function Template Specialization
