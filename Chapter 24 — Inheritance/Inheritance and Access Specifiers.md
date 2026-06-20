---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - syntax
  - best-practice
aliases:
  - protected access specifier
  - public inheritance
  - private inheritance
  - protected inheritance
  - inheritance access
  - inheritance type
up: "[[Chapter 24 — Inheritance]]"
related:
  - "[[Basic Inheritance]]"
  - "[[Hiding Inherited Functionality]]"
  - "[[Member Access]]"
  - "[[Data Hiding]]"
  - "[[Classes]]"
  - "[[Chapter 24 — Inheritance]]"
---

# Inheritance and Access Specifiers

Inheritance interacts with C++'s three access specifiers in two ways: the **`protected` keyword** controls member visibility between base and derived classes, and the **inheritance type** (`public`/`protected`/`private`) determines how inherited members are re-exposed in the derived class.

## The `protected` access specifier

`protected` is a third access level that sits between `public` and `private`. It grants access to:
- The class itself and its friends.
- Any **derived class** (and its friends).
- But **not** to the general public.

```cpp
class Base
{
public:
    int m_public {};    // accessible by anybody
protected:
    int m_protected {}; // accessible by Base members, friends, and derived classes
private:
    int m_private {};   // accessible only by Base members and friends
};
```

See [[Member Access]] for the full access-specifier model. `protected` is the addition that [[Data Hiding|data hiding]] makes available specifically for inheritance hierarchies.

## Inheritance types

The three inheritance types control how each base member's access level is **re-mapped** in the derived class:

| Access in Base | `public` inheritance | `protected` inheritance | `private` inheritance |
|---|---|---|---|
| `public` | `public` | `protected` | `private` |
| `protected` | `protected` | `protected` | `private` |
| `private` | inaccessible | inaccessible | inaccessible |

```cpp
class Pub : public    Base {};  // most common
class Pro : protected Base {};  // rare
class Pri : private   Base {};  // uncommon
class Def :           Base {};  // defaults to private inheritance
```

### Public inheritance
- Public members stay public; protected members stay protected; private members are inaccessible.
- **Use public inheritance unless you have a specific reason to do otherwise.**
- Models a true **is-a** relationship.

### Private inheritance
- Public and protected members both become **private** in the derived class.
- Useful when you want to reuse implementation without exposing the base interface ("implemented-in-terms-of").

### Protected inheritance
- Public and protected members become **protected**; private members are inaccessible.
- Use very rarely.

## Key rules

1. A class (and its friends) can always access its **own non-inherited members**, regardless of inheritance type.
2. Inheritance type only affects how **inherited** members are re-exposed to the outside and to further-derived classes.
3. Private base members are **always inaccessible** in derived classes, regardless of inheritance type — they exist in the derived object's memory but cannot be named.

## Caveat: virtual functions and access

Hiding an inherited member by changing its access specifier in a derived class does **not** prevent it from being called via a base-class reference or pointer if it's a virtual function. The access check uses the static type, but dispatch uses the dynamic type.

> Full coverage: [[Chapter 24 — Inheritance]] → Inheritance and Access Specifiers
