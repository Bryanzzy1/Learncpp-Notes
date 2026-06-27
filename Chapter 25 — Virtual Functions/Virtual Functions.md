---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - cpp/polymorphism
  - concept
  - syntax
  - best-practice
aliases:
  - virtual function
  - virtual keyword
  - virtual method
  - function override
  - override
  - dynamic dispatch
up: "[[Chapter 25 — Virtual Functions]]"
related:
  - "[[Pointers and References to Base]]"
  - "[[Virtual Destructors]]"
  - "[[Override and Final Specifiers]]"
  - "[[Printing with Virtual Dispatch]]"
  - "[[Polymorphism]]"
  - "[[Virtual Table]]"
  - "[[Early Binding and Late Binding]]"
  - "[[Overriding Inherited Functions]]"
  - "[[Basic Inheritance]]"
  - "[[Constructors]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Virtual Functions

A **virtual function** is a member function that, when called through a base class pointer or reference, resolves to the **most-derived version** of that function for the actual runtime type of the object.

## Syntax

```cpp
class Base
{
public:
    virtual std::string_view getName() const { return "Base"; }
};

class Derived : public Base
{
public:
    virtual std::string_view getName() const { return "Derived"; }
    //      ↑ implicit if Base::getName is virtual; explicit here for clarity
};
```

## Overrides

A derived function is an **override** if it matches the base virtual function in:
- Name
- Parameter types
- `const`-ness
- Return type (with the exception of [[Override and Final Specifiers#Covariant return types|covariant return types]])

Use the `override` specifier (see [[Override and Final Specifiers]]) to have the compiler verify the match.

## Rules and caveats

| Rule | Detail |
|---|---|
| Works only through pointer/reference | Calling `derived.getName()` directly uses static dispatch |
| Implicitly propagates | If the base declares `virtual`, all matching overrides in derived classes are implicitly virtual |
| Never call from constructors/destructors | The vtable is not fully set up during construction/destruction — calling a virtual function there invokes the version for the class currently being constructed/destroyed, not the most-derived one |
| Performance cost | Each call requires a vtable lookup; each object gains one hidden `*__vptr` pointer (see [[Virtual Table]]) |

## How resolution works

Virtual dispatch is implemented via the [[Virtual Table]]. When `getName()` is called on a `Base*`, the CPU reads `*__vptr` for the actual object and jumps to the function entry in that class's vtable — **late binding** rather than the compile-time **static dispatch** used for non-virtual calls. See [[Early Binding and Late Binding]] for the conceptual distinction and [[Pointers and References to Base]] for why static dispatch alone is insufficient.

## Subnodes

- [[Virtual Destructors]] — rules for destructors and when to make them virtual
- [[Printing with Virtual Dispatch]] — pattern for virtual `operator<<` using a delegating `print()` member

> Full coverage: [[Chapter 25 — Virtual Functions]] → Virtual Functions
