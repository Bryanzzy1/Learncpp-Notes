---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - best-practice
aliases:
  - object slicing
  - slicing
  - slice
up: "[[Chapter 25 — Virtual Functions]]"
related:
  - "[[Pointers and References to Base]]"
  - "[[Basic Inheritance]]"
  - "[[Virtual Functions]]"
  - "[[Undefined Behavior]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Object Slicing

**Object slicing** occurs when a `Derived` object is assigned or copied into a `Base`-typed variable (by value). Only the `Base` portion is copied; the derived members are discarded — "sliced off".

```cpp
Derived d{};
Base b{ d }; // slicing: b holds only the Base subobject of d
```

## Why it happens

A `Base` variable has storage for exactly one `Base` worth of data. Assigning a `Derived` to it performs a **copy of the Base subobject only**. Unlike [[Pointers and References to Base|a pointer or reference]], the variable cannot alias the full derived object.

## Most common accident: pass by value

Slicing is easy to trigger accidentally when passing a derived object to a function that takes a `Base` parameter by value:

```cpp
void print(Base b) { b.getName(); } // always calls Base::getName(), never Derived's

Derived d{};
print(d); // d is sliced on the way in
```

The fix is to accept by pointer (`Base*`) or reference (`Base&`), which preserves virtual dispatch.

## Slicing vectors

Storing derived objects in a `std::vector<Base>` causes slicing at insertion:

```cpp
std::vector<Base> v;
v.push_back(derived); // sliced — only the Base part is stored
```

Use a `std::vector<Base*>` or `std::vector<std::unique_ptr<Base>>` to retain polymorphic behavior.

## Relation to virtual functions

Object slicing bypasses [[Virtual Functions]] entirely: after slicing, the stored object *is* a `Base` — there is no derived data and no vtable pointer for the derived type. This is different from calling a non-virtual function through a base *reference*, where the data is still intact but virtual dispatch is simply not used.

> Full coverage: [[Chapter 25 — Virtual Functions]] → Object Slicing
