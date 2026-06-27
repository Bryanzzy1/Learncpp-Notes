---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - syntax
aliases:
  - virtual inheritance
  - virtual base class
  - diamond problem
  - shared base
  - diamond inheritance
up: "[[Chapter 25 — Virtual Functions]]"
related:
  - "[[Multiple Inheritance]]"
  - "[[Basic Inheritance]]"
  - "[[Virtual Table]]"
  - "[[Derived Class Constructors]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Virtual Base Classes

**Virtual base classes** solve the **diamond problem** that arises in [[Multiple Inheritance]]: when two classes inherit from a common base and a third inherits from both, the grandchild would otherwise contain *two* copies of the grandparent's members.

## Syntax

Add `virtual` to the inheritance specifier in each intermediate class:

```cpp
class PoweredDevice { };

class Scanner  : virtual public PoweredDevice { };
class Printer  : virtual public PoweredDevice { };

class Copier : public Scanner, public Printer { };
```

`Copier` now contains exactly **one** `PoweredDevice` subobject, shared by both `Scanner` and `Printer`.

## Construction responsibility

With virtual inheritance, the **most-derived class** (here, `Copier`) is responsible for constructing the virtual base directly — the intermediate classes' attempts to construct it are ignored:

```cpp
Copier(int scanner, int printer, int power)
    : PoweredDevice{ power },        // Copier constructs the shared base
      Scanner{ scanner, power },
      Printer{ printer, power }
{
}
```

## Why the vtable is involved

Without virtual inheritance, the offset from a subobject to its base is a fixed compile-time constant and can be hardcoded. With virtual inheritance, the shared base's position in memory depends on the most-derived type and can vary. To handle this:

- Each class with a virtual base stores the **runtime offset** to the shared base in its [[Virtual Table]].
- Accessing a virtual base member requires reading the vtable, looking up the offset, and adding it to the subobject's address.

This is why classes with virtual bases gain a vtable (and an extra `*__vptr` pointer) **even when they have no virtual functions**.

## Design note

Virtual inheritance adds complexity and overhead. Consider it only when the diamond structure is genuinely necessary and cannot be redesigned using [[Composition]] or [[Multiple Inheritance|mixins]].

> Full coverage: [[Chapter 25 — Virtual Functions]] → Virtual Base Classes
