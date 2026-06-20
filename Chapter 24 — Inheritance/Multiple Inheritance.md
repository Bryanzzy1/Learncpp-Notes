---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - syntax
  - best-practice
aliases:
  - multiple inheritance
  - diamond problem
  - multiple base classes
  - CRTP
  - Curiously Recurring Template Pattern
up: "[[Chapter 24 — Inheritance]]"
related:
  - "[[Basic Inheritance]]"
  - "[[Mixins]]"
  - "[[Derived Class Constructors]]"
  - "[[Function Templates]]"
  - "[[Classes]]"
  - "[[Chapter 24 — Inheritance]]"
---

# Multiple Inheritance

**Multiple inheritance** allows a derived class to inherit members from more than one base class simultaneously.

## Syntax

List all base classes separated by commas, each with their own access specifier:

```cpp
class Teacher : public Person, public Employee
{
private:
    int m_teachesGrade{};

public:
    Teacher(std::string_view name, int age, std::string_view employer, double wage, int teachesGrade)
        : Person{ name, age }, Employee{ employer, wage }, m_teachesGrade{ teachesGrade }
    {
    }
};
```

Each base class is initialized separately in the [[Member Initializer List]].

## The diamond problem

When two base classes themselves inherit from a common ancestor, the derived class ends up with two copies of the ancestor's members. This ambiguity is the **diamond problem**. C++ provides `virtual` inheritance to solve it, but it adds complexity.

## Design guidance

**Avoid multiple inheritance unless the alternatives lead to more complexity.** The diamond problem, ambiguous member names, and intricate construction order make multiple inheritance difficult to maintain. A composition-based design (see [[Composition]]) is usually preferable.

## Subnodes

- [[Mixins]] — a constrained, idiomatic use of multiple inheritance for adding orthogonal properties

> Full coverage: [[Chapter 24 — Inheritance]] → Multiple Inheritance
