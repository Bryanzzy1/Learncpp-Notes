---
tags:
  - cpp/types
  - cpp/syntax
  - concept
  - syntax
  - best-practice
aliases:
  - type inference
  - auto
  - auto keyword
up: "[[Chapter 10 — Type conversion]]"
related:
  - "[[Type Deduction for Functions]]"
  - "[[Functions]]"
  - "[[Constants]]"
  - "[[constexpr]]"
  - "[[Strings]]"
---

# Type Deduction

**Type deduction** (also called **type inference**) lets the compiler deduce the type of a variable from its initializer using the `auto` keyword.

```cpp
auto d { 5.0 };         // double
auto i { 1 + 2 };       // int
auto x { i };           // int
auto sum { add(5, 6) }; // deduced from add()'s return type
```

## Limitations

- Requires an initializer — `auto x;` does not compile.
- Does not work when the initializer's type is `void` or another incomplete type.

## const is dropped

Type deduction strips top-level `const` from the deduced type. Add it back explicitly if needed:

```cpp
const int cx { 5 };
auto y { cx };        // y is int, not const int
const auto z { cx };  // z is const int
```

See [[Constants]] for more on `const`, and [[constexpr]] for `constexpr auto`.

## String literals

```cpp
auto s { "Hello" }; // const char*, not std::string
```

Use the `s` or `sv` literal suffix to get `std::string` or `std::string_view`:

```cpp
using namespace std::string_literals;
auto s { "Hello"s };  // std::string
```

See [[Strings]] for string literal details.

## When to use

- Use `auto` when the exact type does not matter (iterators, lambda captures, long template types).
- Prefer an explicit type when you need a specific type that differs from the initializer's type, or when the type should be visible to the reader.

For `auto` return types in functions, see [[Type Deduction for Functions]].

> Full coverage: [[Chapter 10 — Type conversion]] → Type Deduction
