---
tags:
  - cpp/classes
  - cpp/oop
  - concept
aliases:
  - dependency
  - uses relationship
  - object dependency
up: "[[Chapter 23 — Object Relationships]]"
related:
  - "[[Association]]"
  - "[[Object Composition]]"
  - "[[Classes]]"
  - "[[Chapter 23 — Object Relationships]]"
---

# Dependencies

A **dependency** is the weakest form of object relationship: one object invokes another object's functionality to accomplish a specific task, but there is no persistent structural link between them.

## Distinction from Association

An [[Association]] typically implies a persistent relationship — one object stores a pointer or reference to another and uses it repeatedly. A dependency is more transient: the relationship exists only during a particular operation (e.g., as a function parameter or local variable) and dissolves when the operation ends.

```cpp
class Printer
{
public:
    void print(const Document& doc); // dependency — Printer uses Document but doesn't store it
};
```

`Printer` depends on `Document` (it calls `doc`'s methods) but does not hold a member pointer to it. Once `print()` returns, there is no relationship.

> Full coverage: [[Chapter 23 — Object Relationships]] → Dependencies
