---
tags:
  - cpp/functions
  - concept
  - syntax
  - best-practice
aliases:
  - default argument
  - default parameter
  - default value
up: "[[Chapter 11 — Functions]]"
related:
  - "[[Function Overloading]]"
  - "[[Overload Resolution]]"
  - "[[Functions]]"
  - "[[Header Files]]"
  - "[[Translation Unit]]"
  - "[[Chapter 11 — Functions]]"
---

# Default Arguments

A **default argument** is a fallback value for a function parameter, used when the caller omits that argument.

```cpp
void print(int x, int y = 10, double z = 3.14);

print(5);           // y = 10, z = 3.14
print(5, 20);       // y = 20, z = 3.14
print(5, 20, 1.0);  // all explicit
```

## Syntax rules

- Must use the **equals sign** — parenthesis or brace initialization are not allowed.
- Once a parameter has a default argument, **all parameters to its right** must also have default arguments.
- Default arguments are inserted by the compiler **at the call site**.

## Declaration placement

Default arguments cannot be redeclared, and must appear before their first use in a [[Translation Unit]].

- If the function has a **forward declaration** (especially in a [[Header Files|header file]]), declare the default there.
- Otherwise, declare it in the function definition.

## Defaults are not part of the signature

Default values do not affect [[Function Overloading|differentiation]]; these are three distinct overloads:

```cpp
void print(int x);                  // signature: print(int)
void print(int x, int y = 10);      // signature: print(int, int)
void print(int x, double y = 20.5); // signature: print(int, double)
```

However, a single-argument call `print(5)` is **ambiguous** because both `print(int, int)` and `print(int, double)` are candidates with their second parameter defaulted — see [[Overload Resolution]].

## Interaction with overloading

```cpp
void print(std::string_view s) { std::cout << s << '\n'; }
void print(char c = ' ')       { std::cout << c << '\n'; }
```

A call `print()` with no arguments is fine (uses the `char` overload with default `' '`), but `print('a')` is unambiguous too. Care is needed when defaults and overloads overlap.

## Function pointers

Default arguments do **not** work for functions called through function pointers — the default is a property of the declaration, not the function type.

> Full coverage: [[Chapter 11 — Functions]] → Default Arguments
