---
tags:
  - cpp/oop
  - cpp/polymorphism
  - cpp/memory
  - concept
aliases:
  - vtable
  - virtual table
  - vptr
  - __vptr
  - virtual function table
up: "[[Chapter 25 — Virtual Functions]]"
related:
  - "[[Virtual Functions]]"
  - "[[Early Binding and Late Binding]]"
  - "[[Polymorphism]]"
  - "[[Virtual Base Classes]]"
  - "[[Memory Model]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Virtual Table

The **virtual table** (vtable) is the runtime mechanism C++ uses to implement [[Virtual Functions|virtual function dispatch]]. Every class that has virtual functions (or inherits from one that does) gets its own vtable.

## Structure

- The vtable is a **static array** of function pointers, created by the compiler at compile time.
- Each entry in the array corresponds to one virtual function in the class's interface.
- Each entry points to the **most-derived override** that objects of that class are allowed to call.

```
Base vtable:     [ &Base::getName,    &Base::foo, ... ]
Derived vtable:  [ &Derived::getName, &Base::foo, ... ]
                     ↑ overridden         ↑ not overridden, inherited
```

## The hidden `*__vptr`

The compiler silently inserts a hidden pointer member `*__vptr` into the base class:

- Set automatically when each object is constructed.
- Points to the vtable of the **actual (most-derived) class** of the object.
- Costs one pointer's worth of storage per object.

When a virtual function is called through a pointer or reference, the CPU:
1. Reads `*__vptr` from the object.
2. Looks up the function address at the appropriate vtable slot.
3. Jumps to that address.

This is **late binding / dynamic dispatch** (see [[Early Binding and Late Binding]]).

## Virtual base classes and vtables

[[Virtual Base Classes]] also use the vtable even when there are no virtual functions. The vtable stores the runtime offset to the shared virtual base, since the position of the base subobject can vary depending on the most-derived type.

## Performance implications

| Cost | Source |
|---|---|
| +1 pointer per object | `*__vptr` storage |
| +1 indirection per call | vtable lookup at each virtual call site |
| No inlining | The compiler generally cannot inline a virtual call through a pointer/reference |

For small objects with no other data, the `*__vptr` can noticeably increase size. See [[Memory Model]] for size/alignment context.

> Full coverage: [[Chapter 25 — Virtual Functions]] → Virtual Table
