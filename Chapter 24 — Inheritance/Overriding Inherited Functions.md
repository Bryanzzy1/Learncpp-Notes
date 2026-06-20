---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - syntax
  - best-practice
aliases:
  - function override
  - redefining base function
  - calling base function
  - overload resolution inheritance
  - using declaration inheritance
  - Base::function
up: "[[Chapter 24 — Inheritance]]"
related:
  - "[[Basic Inheritance]]"
  - "[[Adding Functionality to Derived Classes]]"
  - "[[Hiding Inherited Functionality]]"
  - "[[Inheritance and Access Specifiers]]"
  - "[[Member Functions]]"
  - "[[Chapter 24 — Inheritance]]"
---

# Overriding Inherited Functions

A derived class can **redefine** a function inherited from the base class by declaring a function with the same name and signature. The derived version replaces the base version for calls made on a derived object.

## Basic override

```cpp
class Base
{
public:
    void identify() const { std::cout << "Base::identify()\n"; }
};

class Derived : public Base
{
public:
    void identify() const  // overrides Base::identify
    {
        std::cout << "Derived::identify()\n";
    }
};
```

When `identify()` is called on a `Derived` object, `Derived::identify()` runs — the base version is shadowed.

## Calling the base version from the override

To extend (rather than fully replace) the base behavior, call the base function explicitly using a scope-qualified name:

```cpp
void Derived::identify() const
{
    std::cout << "Derived::identify()\n";
    Base::identify(); // explicitly calls Base version
}
```

Calling `identify()` without `Base::` inside a Derived member function would recursively call `Derived::identify()` — the qualifier is required.

## Overload resolution and the `using` declaration

When a derived class declares a function with the same name as a base function (but a different signature), it **hides all base overloads** of that name — not just the matching one. The `using` declaration restores all base overloads to the derived class's overload set:

```cpp
class Base
{
public:
    void print(int)    { std::cout << "Base::print(int)\n"; }
    void print(double) { std::cout << "Base::print(double)\n"; }
};

class Derived : public Base
{
public:
    using Base::print;             // bring all Base::print overloads into scope
    void print(double) { std::cout << "Derived::print(double)\n"; }
};

Derived d{};
d.print(5); // calls Base::print(int) — best match among all visible overloads
```

Without `using Base::print`, `d.print(5)` would also call `Derived::print(double)` (the only visible `print`), performing an implicit conversion from `int` to `double`.

> Full coverage: [[Chapter 24 — Inheritance]] → Overriding Inherited Functions
