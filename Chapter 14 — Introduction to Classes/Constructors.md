---
tags:
  - cpp/classes
  - cpp/constructors
  - concept
  - syntax
  - best-practice
aliases:
  - constructor
  - constructor function
  - implicit conversion constructor
up: "[[Chapter 14 — Introduction to Classes]]"
related:
  - "[[Member Initializer List]]"
  - "[[Default Constructor]]"
  - "[[Delegating Constructors]]"
  - "[[Copy Constructor]]"
  - "[[Converting Constructors]]"
  - "[[Temporary Class Objects]]"
  - "[[Classes]]"
  - "[[Initialization]]"
  - "[[Structs]]"
---

# Constructors

A **constructor** is a special member function that is automatically called after a non-aggregate class type object is created. Its job is to initialize the object into a valid state.

## Rules

- The constructor's name must **exactly match the class name** (including capitalization). For class templates, the template parameters are excluded.
- Constructors have **no return type** (not even `void`).
- **Constructors should not be `const`** — they need to initialize members of the newly created object.

## How constructors are matched

When a non-aggregate class type object is defined, the compiler searches for an accessible constructor matching the provided initialization values:

- If found → memory is allocated, then the constructor is called.
- If not found → **compilation error**.

## Subnodes

- [[Member Initializer List]] — preferred syntax for initializing data members
- [[Default Constructor]] — constructors that accept no arguments; implicit and explicit forms
- [[Delegating Constructors]] — having one constructor call another to reduce duplication

## Implicit conversions

By default, a constructor taking a single argument acts as a **converting constructor** — the compiler can use it to implicitly convert a value of the argument type to the class type. See [[Converting Constructors]] for how to control this with `explicit`.

## Related

- [[Copy Constructor]] — the special constructor for copying an existing object of the same type
- [[Structs]] — aggregates do not use constructors; adding a constructor makes a type non-aggregate

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Constructors
