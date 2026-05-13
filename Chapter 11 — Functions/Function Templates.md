---
tags:
  - cpp/functions
  - cpp/templates
  - concept
  - syntax
  - best-practice
aliases:
  - function template
  - template
  - type template parameter
  - template type
  - template argument deduction
  - primary template
  - function instance
  - specialization
  - abbreviated function template
  - generic type
  - generic function
up: "[[Chapter 11 — Functions]]"
related:
  - "[[Non-type Template Parameters]]"
  - "[[Deleted Functions]]"
  - "[[Function Overloading]]"
  - "[[Functions]]"
  - "[[Header Files]]"
  - "[[One Definition Rule]]"
  - "[[Translation Unit]]"
  - "[[Inline Functions and Variables]]"
  - "[[Type Deduction]]"
  - "[[Type Deduction for Functions]]"
  - "[[Narrowing Conversions]]"
  - "[[Chapter 11 — Functions]]"
---

# Function Templates

A **function template** is a blueprint the compiler uses to generate one or more overloaded functions, each for a different set of actual types. Rather than writing separate `max(int, int)`, `max(double, double)`, etc., you write one template:

```cpp
template <typename T>
T max(T x, T y)
{
    return (x < y) ? y : x;
}
```

`T` is a **type template parameter** (informally a **template type** or **generic type**) — a placeholder resolved at compile time.

## Template instantiation

To call the template: `max<int>(3, 5)` — the compiler generates (instantiates) a concrete `max<int>` function. This is **implicit instantiation**.

- The generated function is technically called a **specialization** (colloquially a **function instance**).
- A template is only instantiated **once per translation unit** per unique argument set; subsequent calls reuse the already-instantiated function.

The original template definition is called the **primary template**.

## Template argument deduction

The compiler can usually deduce `T` from the call arguments:

```cpp
max(3, 5);    // T deduced as int
max<>(3, 5);  // same, but restricts to template overloads only
max<int>(3, 5); // explicit
```

During deduction, the compiler does **no type conversions** — both arguments must independently resolve to the same type. Prefer the plain call syntax unless you specifically need to force the template overload over a non-template one.

## Multiple template type parameters

When arguments may differ in type, use two parameters:

```cpp
template <typename T, typename U>
auto max(T x, U y)
{
    return (x < y) ? y : x;
}
```

Using `auto` as the return type lets the compiler pick the correct type; the returned value would otherwise risk a [[Narrowing Conversions|narrowing conversion]]. If the function needs a forward declaration, spell out the return type explicitly.

## Abbreviated function templates (C++20)

`auto` parameters in a regular function declaration are sugar for function templates:

```cpp
auto max(auto x, auto y) { return (x < y) ? y : x; }
// equivalent to:
template <typename T, typename U>
auto max(T x, U y) { return (x < y) ? y : x; }
```

Each `auto` parameter becomes an **independent** type template parameter.

## Function templates with non-template parameters

Not every parameter needs to be templated:

```cpp
template <typename T>
int someFcn(T x, double y) { return 5; }
```

The first parameter is generic; the second is always `double`.

## Overloading templates

Function templates can be overloaded. The most restrictive/specialized template wins:

```cpp
template <typename T>       auto add(T x, T y)          { return x + y; }
template <typename T, typename U>  auto add(T x, U y)   { return x + y; }
template <typename T, typename U, typename V> auto add(T x, U y, V z) { return x + y + z; }
```

## Disallowing certain instantiations

Delete specific instantiations just like regular overloads:

```cpp
const char* addOne(const char* x) = delete;
```

Or delete via a template to block all non-explicit types (see [[Deleted Functions]]).

## Templates in multiple files

Put template definitions in **[[Header Files|header files]]**, not `.cpp` files. Templates are exempt from the [[One Definition Rule|ODR]]'s one-definition-per-program rule; the same template definition may be `#included` into multiple translation units. Implicitly instantiated functions are treated as [[Inline Functions and Variables|inline]], so identical definitions across translation units are legal.

See [[Non-type Template Parameters]] for templates parameterized on constexpr values instead of types.

> Full coverage: [[Chapter 11 — Functions]] → Function Templates
