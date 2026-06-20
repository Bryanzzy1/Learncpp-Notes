---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - syntax
aliases:
  - extending derived class
  - adding derived members
  - new derived function
up: "[[Chapter 24 — Inheritance]]"
related:
  - "[[Basic Inheritance]]"
  - "[[Overriding Inherited Functions]]"
  - "[[Classes]]"
  - "[[Member Functions]]"
  - "[[Chapter 24 — Inheritance]]"
---

# Adding Functionality to Derived Classes

A derived class can extend a base class by defining **new member functions and variables** that are not present in the base. No special syntax is required — simply declare them in the derived class body.

```cpp
class Person
{
protected:
    std::string m_name{};
    int m_age{};
};

class Employee : public Person
{
private:
    double m_wage{};   // new member variable

public:
    void work()        // new member function not in Person
    {
        std::cout << m_name << " is working.\n";
    }
};
```

`work()` and `m_wage` exist only in `Employee` — they are not part of `Person`. Code that holds a `Person` reference or pointer cannot call `work()`.

This is the simplest form of reuse through inheritance: inherit everything from the base, then layer additional capabilities on top. For modifying existing base behavior, see [[Overriding Inherited Functions]].

> Full coverage: [[Chapter 24 — Inheritance]] → Adding Functionality to Derived Classes
