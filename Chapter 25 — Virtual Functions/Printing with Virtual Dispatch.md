---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/polymorphism
  - concept
  - syntax
  - subnode
aliases:
  - virtual print
  - printing with inheritance
  - virtual operator<<
  - print delegation pattern
up: "[[Virtual Functions]]"
related:
  - "[[Virtual Functions]]"
  - "[[Operator Overloading]]"
  - "[[Overloading IO Operators]]"
  - "[[Friend Functions]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Printing with Virtual Dispatch

`operator<<` cannot be made `virtual` because it is a **non-member function** (or a `friend`). Virtual dispatch only applies to member functions called through a pointer/reference.

## The delegation pattern

The solution is to write `operator<<` once in the base class and have it **delegate** to a virtual member function that *can* be overridden:

```cpp
class Base
{
public:
    virtual std::string identify() const { return "Base"; }

    friend std::ostream& operator<<(std::ostream& out, const Base& b)
    {
        return out << b.identify(); // virtual dispatch picks the right override
    }
};

class Derived : public Base
{
public:
    std::string identify() const override { return "Derived"; }
};
```

Calling `std::cout << derived_ref` now prints `"Derived"` even through a `Base&`, because virtual dispatch resolves `identify()` at runtime.

## More flexible variant: `print(ostream&)`

A more powerful form passes the `std::ostream&` into the virtual function, giving each override full stream access:

```cpp
class Base
{
public:
    virtual void print(std::ostream& out) const { out << "Base"; }

    friend std::ostream& operator<<(std::ostream& out, const Base& b)
    {
        b.print(out); // delegate; virtual dispatch selects override
        return out;
    }
};
```

This removes the single-string limitation and lets overrides call `operator<<` on their own members (e.g. `out << m_value`).

## See also

- [[Overloading IO Operators]] for the general `operator<<` overloading pattern.
- [[Friend Functions]] for why `operator<<` is typically a `friend`.

> Full coverage: [[Chapter 25 — Virtual Functions]] → Virtual Functions → Printing with Virtual Dispatch
