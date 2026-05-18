---
tags:
  - cpp/types
  - cpp/enums
  - concept
  - syntax
  - subnode
aliases:
  - unscoped enum
  - unscoped enumeration
  - plain enum
up: "[[Enumerations]]"
related:
  - "[[Scoped Enumerations]]"
  - "[[Operator Overloading]]"
  - "[[static_cast]]"
  - "[[Namespaces]]"
  - "[[Initialization]]"
---

# Unscoped Enumerations

An **unscoped enumeration** (declared with `enum`) places its enumerators directly into the enclosing scope and implicitly converts to `int`.

```cpp
enum Color
{
    red,   // 0 — placed into the enclosing (e.g. global) scope
    green, // 1
    blue,  // 2 — trailing comma optional but recommended
};
```

## Enumerator values

Enumerators default to 0, 1, 2, … Explicit values can be assigned:

```cpp
enum Status { ok = 0, error = 1, warning = 2 };
```

Avoid assigning explicit values unless you have a compelling reason. It is safe to [[static_cast]] any integral value in range of the underlying type even if no enumerator represents it.

## Underlying type

The underlying integral type can be specified:

```cpp
enum Color : std::int8_t { red, green, blue };
```

When a base is specified, brace-initialization from an integer is allowed in C++17:

```cpp
Pet pet1 { 2 }; // ok in C++17 when base is specified
Pet pet2 (2);   // compile error
Pet pet3 = 2;   // compile error
pet1 = 3;       // compile error
```

## Scope and naming collisions

Enumerators pollute the enclosing namespace. Prefer placing unscoped enumerations inside a [[Namespaces|named scope]] (namespace or class) to prevent collisions, or switch to [[Scoped Enumerations]].

## Overload resolution with enums

When an unscoped enum is used in a function call or with an operator, the compiler first seeks an overload matching the enum type before falling back to integral conversions. This makes it possible to define `operator<<` for custom enums (see [[Operator Overloading]]).

> Full coverage: [[Chapter 13 — Compound Types]] → Unscoped Enumerations
