---
tags:
  - cpp/templates
  - cpp/types
  - concept
  - syntax
  - best-practice
aliases:
  - class template
  - struct template
  - std::pair
up: "[[Chapter 13 — Compound Types]]"
related:
  - "[[CTAD]]"
  - "[[Alias Templates]]"
  - "[[Structs]]"
  - "[[Function Templates]]"
  - "[[Non-type Template Parameters]]"
  - "[[One Definition Rule]]"
  - "[[Header Files]]"
  - "[[Type Aliases]]"
---

# Class Templates

A **class template** is a template definition used to instantiate class types (structs, classes, or unions) for different type arguments. It is the class-level analog of [[Function Templates|function templates]].

```cpp
template <typename T>
struct Pair
{
    T first{};
    T second{};
};

Pair<int> p1 { 1, 2 };       // T = int
Pair<double> p2 { 1.5, 2.5 }; // T = double
```

## Mixed template and non-template members

Not every member needs to use the template type:

```cpp
template <typename T>
struct Foo
{
    T first{};    // type varies with T
    int second{}; // always int
};
```

## Multiple template type parameters

```cpp
template <typename T, typename U>
struct Pair
{
    T first{};
    U second{};
};
```

## `std::pair`

The standard library provides `std::pair<T, U>` in `<utility>` — equivalent to the multi-type `Pair` above. Prefer `std::pair` over a hand-rolled version.

## Using a class template in a function template

A function template can accept a class template specialization as a parameter:

```cpp
template <typename T>
constexpr T max(Pair<T> p)
{
    return (p.first < p.second ? p.second : p.first);
}
```

Be careful with template parameter shadowing:

```cpp
template <typename Pair>  // ⚠️ "Pair" here is a template param, not the struct
void print(Pair p) { ... }
```

## Template definitions belong in headers

Like [[Function Templates]], class template definitions must be visible in every [[Translation Unit]] that instantiates them. Put them in [[Header Files|header files]]. The [[One Definition Rule]] allows identical template definitions across translation units.

For [[CTAD]] (C++17 deduction) and [[Alias Templates]], see their dedicated notes.

> Full coverage: [[Chapter 13 — Compound Types]] → Class Templates
