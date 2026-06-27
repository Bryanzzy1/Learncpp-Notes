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
  - override specifier
  - final specifier
  - covariant return types
  - covariant
  - override keyword
  - final keyword
up: "[[Chapter 25 — Virtual Functions]]"
related:
  - "[[Virtual Functions]]"
  - "[[Pure Virtual Functions]]"
  - "[[Basic Inheritance]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Override and Final Specifiers

C++11 introduced two **context-sensitive keywords** — `override` and `final` — to make virtual function management safer and more expressive.

## `override`

Place `override` after the function signature on any derived-class function that is intended to override a base virtual:

```cpp
class Derived : public Base
{
public:
    std::string_view getName() const override { return "Derived"; }
    //                                ^^^^^^^^
};
```

**Do not** repeat `virtual` on the override — it is implicit and adding it obscures intent.

`override` causes a **compile error** if:
- The base function with a matching signature does not exist.
- The signature doesn't match exactly (e.g. missing `const`).

This catches typos and signature drift before they become silent runtime bugs.

### `const` ordering

If a function is both `const` and an `override`, `const` must come first:

```cpp
void foo() const override { }  // ✓
void foo() override const { }  // ✗ compile error
```

## `final`

`final` prevents further overriding of a virtual function, or prevents inheriting from a class entirely:

```cpp
class Base
{
public:
    virtual void doSomething() final; // no derived class can override this
};

class MyClass final { };             // no class can inherit from MyClass
```

Use `final` on a class when you do not intend it to be a base class — this also enables certain compiler optimizations (devirtualization).

## Covariant return types

Normally, a virtual function and its override must have identical return types. The exception is **covariant return types**: if the base returns a pointer or reference to class `Base`, the override may return a pointer or reference to a *derived* class:

```cpp
class Base
{
public:
    virtual Base* clone() const;
};

class Derived : public Base
{
public:
    Derived* clone() const override; // covariant — returns Derived* instead of Base*
};
```

This preserves the virtual relationship while making the derived return type available without a cast.

> Full coverage: [[Chapter 25 — Virtual Functions]] → Override and Final Specifiers
