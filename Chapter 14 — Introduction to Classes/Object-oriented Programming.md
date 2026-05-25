---
tags:
  - cpp/oop
  - cpp/classes
  - concept
aliases:
  - OOP
  - object-oriented
  - object oriented programming
up: "[[Chapter 14 — Introduction to Classes]]"
related:
  - "[[Classes]]"
  - "[[Structs]]"
  - "[[Program-defined Types]]"
  - "[[Data Hiding]]"
---

# Object-oriented Programming

**Object-oriented programming (OOP)** is a programming paradigm focused on creating **program-defined data types that bundle both properties (data) and a set of well-defined behaviors (functions)** together.

Prior chapters focused on **procedural programming** — writing functions that operate on separate data. OOP shifts the model: instead of data and functions living separately, they are packaged into **objects** (instances of a class type).

The key ideas OOP adds on top of [[Program-defined Types]] like [[Structs]]:

| Concept | Meaning |
|---------|---------|
| **Encapsulation** | Data is hidden inside the object; users interact through a defined interface |
| **Abstraction** | Users don't need to know how an object works, only how to use it |
| **Invariants** | The object is responsible for keeping itself in a valid state |

In C++, OOP is implemented primarily through **classes** (see [[Classes]]) — which add access control, constructors, and member functions on top of the struct model.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Object-oriented Programming
