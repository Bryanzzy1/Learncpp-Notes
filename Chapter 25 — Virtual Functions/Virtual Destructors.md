---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - best-practice
  - subnode
aliases:
  - virtual destructor
  - virtual assignment
  - ignoring virtualization
up: "[[Virtual Functions]]"
related:
  - "[[Virtual Functions]]"
  - "[[Destructors]]"
  - "[[Basic Inheritance]]"
  - "[[Dynamic Casting]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Virtual Destructors

When an object is deleted through a base class pointer and the destructor is not virtual, only the **base class destructor** runs — the derived destructor is skipped, causing resource leaks or [[Undefined Behavior]]:

```cpp
Base* p{ new Derived{} };
delete p; // calls Base::~Base() only if ~Base is not virtual — UB if Derived has its own destructor
```

Making the base destructor `virtual` ensures the correct (most-derived) destructor is called:

```cpp
class Base
{
public:
    virtual ~Base() = default; // virtual destructor
};
```

## Rules of thumb

| Situation | Rule |
|---|---|
| Class is designed as a base class or has any virtual function | Give it a `virtual` destructor |
| Class is not designed to be a base class | No virtual members, no virtual destructor |

Herb Sutter's guideline: **"A base class destructor should be either public and virtual, or protected and non-virtual."**

- **Public + virtual**: anyone can `delete` through a base pointer safely.
- **Protected + non-virtual**: prevents `delete` through a base pointer entirely (the public can't call a protected destructor), eliminating the hazard — but also makes the base impractical to use directly.

## Virtual assignment

Virtualizing `operator=` opens complex correctness issues (signature mismatches between base and derived forms) and is **not recommended**. Leave assignment operators non-virtual.

## Ignoring virtualization

Occasionally you need to call a specific base-class version instead of the virtual override. Use the scope resolution operator:

```cpp
Base::doSomething(); // explicitly calls Base version, skipping virtual dispatch
```

This is rarely needed and should be used sparingly.

> Full coverage: [[Chapter 25 — Virtual Functions]] → Virtual Functions → Virtual Destructors
