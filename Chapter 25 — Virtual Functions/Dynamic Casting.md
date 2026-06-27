---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - cpp/polymorphism
  - concept
  - syntax
  - best-practice
aliases:
  - dynamic_cast
  - downcasting
  - RTTI
  - run-time type information
  - runtime type information
  - std::bad_cast
up: "[[Chapter 25 — Virtual Functions]]"
related:
  - "[[Virtual Functions]]"
  - "[[static_cast]]"
  - "[[Pointers and References to Base]]"
  - "[[Polymorphism]]"
  - "[[Undefined Behavior]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Dynamic Casting

`dynamic_cast` is a C++ casting operator that safely converts a base class pointer or reference to a derived class pointer or reference (**downcasting**).

## Syntax

```cpp
Base* b{ getObject() };

Derived* d{ dynamic_cast<Derived*>(b) }; // downcast pointer
if (d)
{
    // safe to use d
}
```

For references:

```cpp
Derived& d{ dynamic_cast<Derived&>(rBase) }; // throws std::bad_cast on failure
```

## Failure behavior

| Cast type | On failure |
|---|---|
| Pointer (`T*`) | Returns `nullptr` |
| Reference (`T&`) | Throws `std::bad_cast` |

Always check the result of a pointer `dynamic_cast` before using it.

## How it works: RTTI

`dynamic_cast` relies on **Run-time Type Information (RTTI)** — metadata the compiler attaches to polymorphic types (classes with at least one virtual function). RTTI stores the actual type of an object so it can be inspected at runtime.

Some compilers allow disabling RTTI as an optimization (`/GR-` in MSVC, `-fno-rtti` in GCC/Clang). If RTTI is disabled, `dynamic_cast` does not function correctly.

## `dynamic_cast` vs `static_cast` for downcasting

| | `dynamic_cast` | `static_cast` |
|---|---|---|
| Runtime type check | Yes | No |
| On wrong type | Returns `nullptr` / throws | [[Undefined Behavior]] |
| Speed | Slower | Faster |
| Safety | Safe | Dangerous |

Use `static_cast` for downcasting **only** when you are certain (from external logic) that the pointer points to the target type.

## Design preference

Prefer **virtual functions** over `dynamic_cast`. If you find yourself casting down to call a derived-specific method, that method usually belongs in the base class interface (possibly as a pure virtual). `dynamic_cast` is appropriate when downcasting is genuinely necessary and the design cannot avoid it.

> Full coverage: [[Chapter 25 — Virtual Functions]] → Dynamic Casting
