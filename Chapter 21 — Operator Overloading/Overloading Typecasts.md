---
tags:
  - cpp/operators
  - cpp/classes
  - cpp/type-conversion
  - concept
  - syntax
  - best-practice
aliases:
  - typecast operator overload
  - conversion operator
  - overloaded cast
  - explicit conversion operator
  - user-defined conversion
up: "[[Chapter 21 — Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Member Function Overloads]]"
  - "[[Converting Constructors]]"
  - "[[Implicit Type Conversion]]"
  - "[[static_cast]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Overloading Typecasts

A **typecast operator overload** (also called a **conversion operator**) defines how a class converts to another type. Like [[Implicit Type Conversion|implicit conversions]], it can be invoked automatically by the compiler, or explicitly with a cast.

## Syntax

```cpp
class Cents
{
public:
    // Overloaded int cast — converts Cents to int
    operator int() const { return m_cents; }
private:
    int m_cents{};
};

Cents c{ 5 };
int x{ static_cast<int>(c) }; // explicit
int y{ c };                    // implicit — compiler uses operator int()
```

- No return type is specified in the declaration (the type being converted to serves as the return type).
- Must be a **member function** — there is no left operand.

## Converting to program-defined types

You can define conversions to your own types, not just built-ins:

```cpp
class Dollars
{
public:
    operator Cents() const { return Cents{ m_dollars * 100 }; }
private:
    int m_dollars{};
};
```

## Mark as `explicit` (best practice)

Just as [[Converting Constructors|converting constructors]] should be `explicit`, typecast operators should be too — unless the destination type is essentially synonymous with the source (e.g. a thin wrapper):

```cpp
explicit operator int() const { return m_cents; }
```

With `explicit`, the conversion only fires on a direct cast (`static_cast<int>(c)`), not silently during implicit conversions.

## Choosing between converting constructors and typecast operators

When converting type A → type B:

| Condition | Prefer |
|---|---|
| B is a class you can modify | [[Converting Constructors|Converting constructor]] in B |
| A is a class you can modify, but not B | Typecast operator in A |
| Neither A nor B is yours | Non-member conversion function |

In general, prefer [[Converting Constructors|converting constructors]] over typecast operators — constructors are often more discoverable and controllable.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Overloading Typecasts
