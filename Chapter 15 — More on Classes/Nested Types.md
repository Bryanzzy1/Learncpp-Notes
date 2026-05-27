---
tags:
  - cpp/classes
  - cpp/types
  - concept
  - syntax
aliases:
  - nested type
  - member type
  - nested class
  - nested enum
  - nested typedef
up: "[[Chapter 15 — More on Classes]]"
related:
  - "[[Classes]]"
  - "[[Enumerations]]"
  - "[[Scoped Enumerations]]"
  - "[[Unscoped Enumerations]]"
  - "[[Member Access]]"
  - "[[Type Aliases]]"
  - "[[this Pointer]]"
---

# Nested Types

A **nested type** (also called a **member type**) is a type defined inside a class definition, under an access specifier. Nested types are scoped to the enclosing class.

## Nested enumerations

The most common use case is scoping an enumeration to the class that owns it, so callers use `ClassName::EnumValue` rather than a global name:

```cpp
class Fruit
{
public:
    enum Type   // unscoped enum, nested inside Fruit
    {
        apple,
        banana,
        cherry
    };

private:
    Type m_type{};

public:
    Fruit(Type type) : m_type{ type } {}
    Type getType() { return m_type; }

    bool isCherry() { return m_type == cherry; } // no prefix needed inside member functions
};

Fruit f { Fruit::apple }; // caller must use Fruit::apple
```

Prefer [[Scoped Enumerations|scoped enumerations]] (`enum class`) when you want stronger type safety.

## Nested type aliases

A class can define `typedef` or `using` aliases as member types:

```cpp
class MyContainer
{
public:
    using IDType = int; // nested type alias
};

MyContainer::IDType id { 42 };
```

## Nested classes

A class can contain another class as a nested type. Key rules:

- The nested class **does not have access to `this`** of the outer class — it cannot directly access the outer class's instance members.
- Because the nested class is a *member* of the outer class, it **can access private static members** of the outer class that are in scope.

## Forward declarations of nested types

A nested type **can** be forward-declared within its enclosing class. However, a nested type **cannot** be forward-declared *before* the enclosing class is defined — the enclosing class definition must come first.

> Full coverage: [[Chapter 15 — More on Classes]] → Nested Types
