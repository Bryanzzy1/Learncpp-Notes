---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
aliases:
  - base class pointer
  - base class reference
  - static dispatch
  - compile-time dispatch
  - type-based dispatch
up: "[[Chapter 25 — Virtual Functions]]"
related:
  - "[[Virtual Functions]]"
  - "[[Basic Inheritance]]"
  - "[[Overriding Inherited Functions]]"
  - "[[Polymorphism]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Pointers and References to Base

A pointer or reference of type `Base` can legally bind to a `Derived` object, because every `Derived` contains a `Base` subobject:

```cpp
Derived derived{};
Base& rBase{ derived };   // lvalue reference to the Base subobject
Base* pBase{ &derived };  // pointer to the Base subobject
```

## Static (compile-time) dispatch

When a member function is called through a `Base` pointer or reference, the call is resolved based on **the type of the pointer/reference**, not the actual runtime type of the underlying object:

```cpp
rBase.getName(); // calls Base::getName(), even if derived overrides it
```

`Derived::getName()` *shadows* `Base::getName()` only for `Derived`-typed access. Through a `Base&` or `Base*`, the compiler sees only the base interface — this is called **static dispatch** (or **early binding**).

## Why this matters

This behavior is the direct motivation for [[Virtual Functions]]. Without `virtual`, there is no way to call a derived-class override through a base-class pointer — the override is invisible to static dispatch. With `virtual`, the call is resolved at runtime based on the actual object type ([[Early Binding and Late Binding]]).

This also means [[Object Slicing]] can occur silently when a `Derived` is copied *by value* into a `Base` variable — only the `Base` portion survives.

> Full coverage: [[Chapter 25 — Virtual Functions]] → Pointers and References to Base
