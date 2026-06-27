---
tags:
  - cpp/oop
  - cpp/polymorphism
  - concept
aliases:
  - early binding
  - late binding
  - static binding
  - dynamic dispatch
  - function binding
  - method binding
  - static dispatch
up: "[[Chapter 25 — Virtual Functions]]"
related:
  - "[[Virtual Functions]]"
  - "[[Virtual Table]]"
  - "[[Polymorphism]]"
  - "[[Function Pointers]]"
  - "[[Pointers and References to Base]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Early Binding and Late Binding

**Binding** is the process of associating a name (a function call) with a specific function definition. **Dispatching** is the act of actually invoking that bound function.

## Early binding (static binding / static dispatch)

When the compiler can determine which function to call at compile time, it uses **early binding**:

- Direct calls to non-member functions.
- Calls to non-virtual member functions.
- Virtual function calls on an object (not pointer/reference) — the type is known at compile time.

The compiler (or linker) emits a direct `CALL` or `JMP` instruction to a fixed function address. This is the fastest possible dispatch.

```cpp
Derived d{};
d.getName(); // early binding — type is known, no vtable needed
```

## Late binding (dynamic dispatch)

When the function to call can only be determined at runtime, **late binding** is used. In C++, this occurs in two main ways:

1. **Function pointers** — the pointer can point to different functions at runtime.
2. **Virtual function dispatch** — the call is resolved via the [[Virtual Table]] based on the actual object type.

```cpp
Base* p{ &derived };
p->getName(); // late binding — reads *__vptr and jumps via vtable entry
```

Late binding is slightly less efficient: the CPU must read the vtable pointer, look up the function address in the table, and then jump — one extra level of indirection.

## Terminology map

| Term | Meaning |
|---|---|
| Early binding / static binding / static dispatch | Compile-time function resolution |
| Late binding | Runtime resolution via indirection (function pointer or vtable) |
| Dynamic dispatch | Specifically: virtual function override resolution via vtable |

Dynamic dispatch is the mechanism that enables [[Polymorphism|runtime polymorphism]].

> Full coverage: [[Chapter 25 — Virtual Functions]] → Early Binding and Late Binding
