---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - syntax
aliases:
  - hiding base member
  - using declaration private
  - delete inherited function
  - restrict inherited access
  - inaccessible inherited member
up: "[[Chapter 24 — Inheritance]]"
related:
  - "[[Basic Inheritance]]"
  - "[[Inheritance and Access Specifiers]]"
  - "[[Overriding Inherited Functions]]"
  - "[[Member Access]]"
  - "[[Chapter 24 — Inheritance]]"
---

# Hiding Inherited Functionality

A derived class can restrict access to inherited members in two ways: by changing a member's access specifier via a `using` declaration, or by deleting a function with `= delete`.

## Restricting access with a `using` declaration

Placing a `using` declaration under a more restrictive access specifier makes the inherited member less accessible in the derived class:

```cpp
class Derived : public Base
{
private:
    using Base::m_value; // m_value was public/protected in Base; now private in Derived

public:
    Derived(int value) : Base{ value } {}
};
```

This is useful when a derived class wants to use base implementation internally while hiding it from outside users.

## Deleting inherited functions

Mark an inherited function as deleted in the derived class to make it completely inaccessible:

```cpp
int getValue() const = delete; // callers cannot call this on Derived objects
```

## Caveat: virtual functions bypass access restrictions

Changing the access specifier of a **virtual** base function in the derived class does not truly prevent access through the base interface. A caller with a `Base&` or `Base*` can still invoke the function — the access check uses the **static type** (Base), while dispatch uses the **dynamic type** (Derived):

```cpp
class A
{
public:
    virtual void fun() { std::cout << "public A::fun()\n"; }
};

class B : public A
{
private:
    virtual void fun() { std::cout << "private B::fun()\n"; }
};

B b{};
A& ref = b;
ref.fun(); // calls B::fun() — access check on A sees public; dispatch goes to B
```

This means `private` on a virtual override is not an effective access restriction when callers hold a base-type reference.

> Full coverage: [[Chapter 24 — Inheritance]] → Hiding Inherited Functionality
