---
tags:
  - cpp/operators
  - cpp/functions
  - concept
  - syntax
aliases:
  - operator overloading
  - overloaded operator
  - operator<<
  - operator>>
up: "[[Chapter 13 — Compound Types]]"
related:
  - "[[Enumerations]]"
  - "[[Unscoped Enumerations]]"
  - "[[Scoped Enumerations]]"
  - "[[Structs]]"
  - "[[Function Overloading]]"
  - "[[Pass by Reference]]"
---

# Operator Overloading

**Operator overloading** lets you define the behavior of a built-in operator (like `<<`, `>>`, `+`, `==`) for user-defined types. The overload is just a function whose name is the operator keyword.

## Rules for writing an overload

1. Name the function `operator<symbol>` (e.g. `operator<<`).
2. Add one parameter per operand, left-to-right. **At least one parameter must be a user-defined type** (class or enum); overloading for purely built-in types is not allowed.
3. Set the return type to whatever makes sense for the operator.
4. Return the result.

## Overloading `operator<<` for output

The most common use in early chapters is teaching `std::cout` to print a custom type:

```cpp
std::ostream& operator<<(std::ostream& out, Color color)
{
    out << getColorName(color);
    return out; // operator<< conventionally returns its left operand
}

int main()
{
    Color shirt { blue };
    std::cout << "Your shirt is " << shirt << '\n';
}
```

- The first parameter is `std::ostream&` (the stream, e.g. `std::cout`).
- The second parameter is the user-defined type being printed.
- Both parameters are **references** (via [[Pass by Reference]]) to avoid copies.
- Returning `out` enables chaining (`std::cout << a << b`).

## Overloading `operator>>` for input

Mirrors `operator<<` but takes `std::istream&` and modifies the user type via a non-const reference:

```cpp
std::istream& operator>>(std::istream& in, Color& color)
{
    // read and assign to color
    return in;
}
```

## Relationship to enumerations

Unscoped and scoped enums don't print meaningfully by default. Overloading `operator<<` is the idiomatic way to give them readable output without losing type safety (see [[Unscoped Enumerations]] and [[Scoped Enumerations]]).

> Full coverage: [[Chapter 13 — Compound Types]] → Operator Overloading
