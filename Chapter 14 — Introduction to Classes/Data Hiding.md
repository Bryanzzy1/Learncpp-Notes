---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - best-practice
aliases:
  - encapsulation
  - information hiding
  - data abstraction
  - class interface
  - public interface
up: "[[Chapter 14 — Introduction to Classes]]"
related:
  - "[[Classes]]"
  - "[[Member Access]]"
  - "[[Structs]]"
  - "[[Program-defined Types]]"
---

# Data Hiding

**Data hiding** (also called **information hiding** or **data abstraction**) is the technique of enforcing a separation between a class's **interface** and its **implementation** — the implementation details are hidden from users.

## Interface vs implementation

The **class interface** defines how a user interacts with objects of the class type. When the interface is composed of public members, it's called the **public interface**.

The **implementation** is the private data and internal logic that makes the interface work. Users don't need to know (and shouldn't depend on) the implementation.

## Encapsulation

**Encapsulation** refers to either:
- Enclosing one or more items within a container (the structural sense)
- Bundling data and the functions that operate on it together (the OOP sense, as in [[Classes]])

To use an encapsulated class, you only need to know its public interface — what member functions are available, what arguments they take, and what they return. The internal [[Member Access|private members]] are inaccessible.

## Declaration order recommendation

Declare members in this order to emphasize the interface:

1. `public` members first
2. `protected` members next
3. `private` members last

This makes the public interface immediately visible to anyone reading the class definition.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Data Hiding
