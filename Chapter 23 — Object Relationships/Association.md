---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - best-practice
aliases:
  - association
  - uses-a relationship
  - reflexive association
  - bidirectional association
up: "[[Chapter 23 — Object Relationships]]"
related:
  - "[[Object Composition]]"
  - "[[Composition]]"
  - "[[Aggregation]]"
  - "[[Dependencies]]"
  - "[[Classes]]"
  - "[[Chapter 23 — Object Relationships]]"
---

# Association

**Association** is an object relationship where two otherwise unrelated objects interact with or use each other. Unlike [[Composition]] and [[Aggregation]], the associated object is not a "part" of the class — it is simply used by it.

## Qualifying rules

All four conditions must hold:

1. The associated object is **otherwise unrelated** to the containing object.
2. The associated object can **belong to more than one object** at a time.
3. The associated object's **existence is not managed** by the class.
4. The associated object **may or may not know** about the existence of the class (can be unidirectional or bidirectional).

## Relationship verb: "uses-a"

Association represents a **uses-a** relationship — one class uses the services of another without owning it or being composed of it.

## Key distinctions from composition/aggregation

| Property | Composition | Aggregation | Association |
|---|---|---|---|
| Relationship type | Whole/part | Whole/part | Otherwise unrelated |
| Members can belong to multiple classes | No | Yes | Yes |
| Members' existence managed by class | Yes | No | No |
| Directionality | Unidirectional | Unidirectional | Unidirectional or **bidirectional** |
| Relationship verb | Part-of | Has-a | **Uses-a** |

## Indirect association

Associations do not have to be implemented via pointer members. A class can associate with another through a parameter, a local variable, or any other mechanism that does not persist beyond the function call.

## Reflexive association

An object may have an association with **another object of its own type**. For example, an `Employee` might have a manager who is also an `Employee`. This is a **reflexive association**:

```cpp
class Employee
{
    std::string m_name;
    const Employee* m_manager{}; // reflexive — points to another Employee
};
```

> Full coverage: [[Chapter 23 — Object Relationships]] → Association
