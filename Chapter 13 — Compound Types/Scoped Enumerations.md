---
tags:
  - cpp/types
  - cpp/enums
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - scoped enum
  - scoped enumeration
  - enum class
  - enum struct
up: "[[Enumerations]]"
related:
  - "[[Unscoped Enumerations]]"
  - "[[Operator Overloading]]"
  - "[[static_cast]]"
  - "[[Type Aliases]]"
---

# Scoped Enumerations

A **scoped enumeration** (declared with `enum class` or `enum struct`) fixes the two main problems with [[Unscoped Enumerations]]:

1. Enumerators are scoped to the enum — they do **not** leak into the enclosing namespace.
2. They do **not** implicitly convert to integers.

```cpp
enum class Color { red, blue };
enum class Fruit { banana, apple };

Color color { Color::red };     // must qualify with Color::
Fruit fruit { Fruit::banana };

if (color == fruit) { }  // compile error: different types, no implicit conversion
```

**Favor scoped enumerations over unscoped enumerations** unless there is a compelling reason to do otherwise.

## Converting to the underlying type

Scoped enums require an explicit conversion to their underlying integer:

```cpp
// C++23: std::to_underlying (from <utility>)
std::cout << std::to_underlying(Color::red);

// Pre-C++23: static_cast
std::cout << static_cast<int>(Color::red);
```

A common pattern to reduce verbosity is to overload `operator+`:

```cpp
template <typename T>
constexpr auto operator+(T a) noexcept
{
    return static_cast<std::underlying_type_t<T>>(a);
}

std::cout << +Animals::elephant << '\n'; // convert via unary +
```

## `using enum` (C++20)

A `using enum` statement imports all enumerators from an enum into the current scope — useful inside a `switch` or a class:

```cpp
using enum Color;
std::cout << red; // no need for Color::red
```

> Full coverage: [[Chapter 13 — Compound Types]] → Scoped Enumerations
