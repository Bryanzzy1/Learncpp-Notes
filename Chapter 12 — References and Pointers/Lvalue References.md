---
tags:
  - cpp/references
  - concept
  - syntax
aliases:
  - lvalue reference
  - reference
  - reference type
  - reference binding
  - referent
  - dangling reference
  - referenced type
up: "[[Chapter 12 — References and Pointers]]"
related:
  - "[[Lvalue References to Const]]"
  - "[[Pass by Reference]]"
  - "[[Value Categories]]"
  - "[[Pointers]]"
  - "[[Undefined Behavior]]"
  - "[[Initialization]]"
---

# Lvalue References

A **reference** is an alias for an existing object. Any operation on the reference is applied to the object it refers to.

## Reference type syntax

```cpp
int        // normal int (not a reference)
int&       // lvalue reference to int
double&    // lvalue reference to double
const int& // lvalue reference to const int
```

The type `int&` is called a **reference type**; `int` is the **referenced type**. An lvalue reference to a non-const is often just called an "lvalue reference"; with `const`, it's a **lvalue reference to const** (see [[Lvalue References to Const]]).

## Initialization and binding

References use [[Initialization|reference initialization]]: when initialized with an object (the **referent**), the reference is said to be **bound** to it (**reference binding**).

**Non-const lvalue references can only bind to modifiable lvalues** — they cannot bind to rvalues or const objects. The reference type must match the referenced type (with some inheritance exceptions).

```cpp
int x { 5 };
int& ref { x }; // ref is bound to x

x = 6;          // x now 6; ref also reads 6
ref = 7;        // modifies x through ref; both now 7
```

## References are not objects

A reference is not itself an object — it has no independent address. Binding a second reference to an existing reference just binds both to the same underlying variable:

```cpp
int var{};
int& ref1{ var };  // bound to var
int& ref2{ ref1 }; // also bound to var (not a reference-to-reference)
```

## References cannot be reseated

Once bound, a reference always refers to the same object for its entire lifetime. Assigning through a reference changes the referent's value, not which object the reference points to.

## Lifetimes and dangling references

A reference and its referent have **independent lifetimes**. If the referent is destroyed before the reference, the reference becomes a **dangling reference** — using it is [[Undefined Behavior]].

For const lvalue references and their interaction with rvalues and temporaries, see [[Lvalue References to Const]]. For passing by reference in function calls, see [[Pass by Reference]].

> Full coverage: [[Chapter 12 — References and Pointers]] → Lvalue References
