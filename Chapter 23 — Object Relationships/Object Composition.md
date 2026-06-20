---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - best-practice
aliases:
  - object composition
  - composite type
  - composition vs aggregation
up: "[[Chapter 23 — Object Relationships]]"
related:
  - "[[Composition]]"
  - "[[Aggregation]]"
  - "[[Association]]"
  - "[[Dependencies]]"
  - "[[Classes]]"
  - "[[Structs]]"
  - "[[Chapter 23 — Object Relationships]]"
---

# Object Composition

**Object composition** is the process of building complex objects from simpler ones by including instances of other types as members. [[Classes]] and [[Structs]] that contain member objects are sometimes called **composite types**.

Object composition describes a **has-a** or **part-of** relationship. There are two subtypes with different ownership rules:

| | [[Composition]] | [[Aggregation]] |
|---|---|---|
| Implementation | Normal member variables (or owning pointers) | Pointer or reference members pointing outside the class |
| Part lifetime managed by class? | Yes | No |
| Part can belong to multiple classes? | No | Yes |
| Relationship verb | Part-of | Has-a |

## Choosing between them

Prefer the **simplest relationship type** that meets your program's needs — don't model real-world ownership semantics if your program doesn't require it.

Both composition and aggregation are unidirectional: the part (member) does not know about the existence of the containing class.

> Full coverage: [[Chapter 23 — Object Relationships]] → Object Composition
