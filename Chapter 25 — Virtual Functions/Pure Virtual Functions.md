---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/polymorphism
  - concept
  - syntax
  - best-practice
aliases:
  - pure virtual function
  - abstract function
  - abstract class
  - abstract base class
  - "= 0"
up: "[[Chapter 25 — Virtual Functions]]"
related:
  - "[[Interface Classes]]"
  - "[[Virtual Functions]]"
  - "[[Virtual Destructors]]"
  - "[[Classes]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Pure Virtual Functions

A **pure virtual function** declares that derived classes *must* provide an implementation. It has no body in the base class and is marked with `= 0`:

```cpp
virtual int getValue() const = 0; // pure virtual function
```

## Abstract base classes

Any class containing at least one pure virtual function becomes an **abstract base class**:
- It **cannot be instantiated** directly.
- Derived classes must override *all* pure virtual functions; otherwise they too are abstract.

Abstract base classes serve as contracts — they define what derived classes must do without specifying how.

## Pure virtual functions with definitions

A pure virtual function *can* optionally have an out-of-class body:

```cpp
class Animal
{
public:
    virtual std::string_view speak() const = 0; // pure virtual
    virtual ~Animal() = default;
};

std::string_view Animal::speak() const
{
    return "..."; // optional default implementation
}
```

Derived classes may call this default explicitly via the scope resolution operator:

```cpp
class Dragonfly : public Animal
{
public:
    std::string_view speak() const override
    {
        return Animal::speak(); // opt into the default
    }
};
```

This pattern forces derived classes to actively choose the default rather than silently inheriting it.

## Vtable behavior

The vtable entry for a pure virtual function typically holds a null pointer or points to a placeholder (`__purecall`) that aborts if called. Calling a pure virtual function through a base class pointer (before the derived override is set up) is [[Undefined Behavior]].

## Subnode

- [[Interface Classes]] — a class where *all* functions are pure virtual (no member variables)

> Full coverage: [[Chapter 25 — Virtual Functions]] → Pure Virtual Functions
