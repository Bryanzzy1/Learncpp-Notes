---
tags:
  - cpp/operators
  - cpp/classes
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - normal operator overload
  - non-member operator overload
  - overloading with normal functions
up: "[[Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Friend Function Overloads]]"
  - "[[Member Function Overloads]]"
  - "[[Operator Overload Selection Guide]]"
  - "[[Data Hiding]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Normal Function Overloads

Overloading an operator as a **normal (non-member, non-friend) function** implements the operator using only the class's public interface. No `friend` declaration is needed.

```cpp
class Cents
{
public:
    int getCents() const { return m_cents; }
private:
    int m_cents{};
};

// Normal function — uses the public accessor, no private access needed
Cents operator+(const Cents& c1, const Cents& c2)
{
    return Cents{ c1.getCents() + c2.getCents() };
}
```

## Advantages over friend functions

- No coupling to the class's private implementation — adding or renaming private members does not require updating the operator.
- Better respects [[Data Hiding|encapsulation]].

## When normal functions aren't enough

If the operator genuinely needs access to private members and no suitable public accessor exists, use a [[Friend Function Overloads|friend function]] instead of adding an accessor purely for the operator.

## Best practice

**Prefer normal function overloads over friend function overloads** whenever the public interface is sufficient. This is the least privileged approach and keeps the operator decoupled from internal implementation details.

See [[Operator Overload Selection Guide]] for the full decision matrix.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Operator Overloading → Normal Function Overloads
