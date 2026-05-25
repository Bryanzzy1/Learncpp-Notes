---
tags:
  - cpp/classes
  - cpp/constructors
  - concept
  - syntax
aliases:
  - temporary object
  - anonymous object
  - unnamed object
up: "[[Chapter 14 — Introduction to Classes]]"
related:
  - "[[Classes]]"
  - "[[Constructors]]"
  - "[[Copy Constructor]]"
  - "[[Initialization]]"
  - "[[List Initialization]]"
  - "[[static_cast]]"
  - "[[Return by Value]]"
---

# Temporary Class Objects

A **temporary object** (also called an **anonymous object** or **unnamed object**) is an object that has no name and exists only for the duration of a single expression.

## Creating and passing temporaries

```cpp
void print(IntPair p)
{
    std::cout << "(" << p.x() << ", " << p.y() << ")\n";
}

int main()
{
    // Case 1: Pass a named variable
    IntPair p { 3, 4 };
    print(p);

    // Case 2: Construct a temporary and pass it directly
    print(IntPair { 5, 6 });

    // Case 3: Implicitly convert braced list to a temporary IntPair
    print( { 7, 8 } );
}
```

There is **no syntax** to create a temporary object using copy initialization (e.g. `IntPair = { 5, 6 }` is not valid for temporaries).

## Function return values are temporaries

When a function returns by value (see [[Return by Value]]), the returned object is a temporary, initialized from the `return` expression.

## Choosing between `static_cast` and a list-initialized temporary

| Situation | Prefer |
|-----------|--------|
| Narrowing conversion needed | `static_cast<T>(value)` |
| Making a surprising type change obvious | `static_cast<T>(value)` |
| Want direct-initialization (avoid list constructors) | `static_cast<T>(value)` |
| Need narrowing *protection* | `Type { value }` (list initialization) |
| Constructor requires multiple arguments | `Type { a, b }` |
| Converting to a class type | `Type { value }` |

Use [[static_cast]] when converting to a fundamental type; use [[List Initialization|list-initialized]] temporaries when converting to a class type.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Temporary Class Objects
