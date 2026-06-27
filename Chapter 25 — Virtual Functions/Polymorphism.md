---
tags:
  - cpp/oop
  - cpp/polymorphism
  - concept
aliases:
  - polymorphism
  - compile-time polymorphism
  - runtime polymorphism
  - many forms
up: "[[Chapter 25 — Virtual Functions]]"
related:
  - "[[Virtual Functions]]"
  - "[[Early Binding and Late Binding]]"
  - "[[Virtual Table]]"
  - "[[Object-oriented Programming]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Polymorphism

**Polymorphism** ("many forms") is the ability of an entity to take on multiple forms. In C++, it divides into two categories based on *when* the resolution happens.

## Compile-time polymorphism

Resolved by the compiler before the program runs:
- **Function overload resolution** — the compiler picks the best matching overload based on argument types.
- **Template instantiation** — the compiler generates concrete code for each type argument.

## Runtime polymorphism

Resolved while the program executes:
- **Virtual function resolution** — the call is dispatched to the most-derived override based on the actual runtime type of the object.

Runtime polymorphism in C++ is implemented via the [[Virtual Table]] and is conceptually described as **late binding** or **dynamic dispatch** (see [[Early Binding and Late Binding]]).

## Relationship to OOP

Polymorphism is one of the four pillars of [[Object-oriented Programming]] (alongside encapsulation, inheritance, and abstraction). In practice, it is the mechanism that lets you write code against a base class interface and have derived-class behavior automatically substituted at runtime.

> Full coverage: [[Chapter 25 — Virtual Functions]] → Polymorphism
