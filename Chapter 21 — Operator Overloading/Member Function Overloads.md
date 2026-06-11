---
tags:
  - cpp/operators
  - cpp/classes
  - concept
  - syntax
  - subnode
aliases:
  - member operator overload
  - overloading with member functions
up: "[[Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Friend Function Overloads]]"
  - "[[Normal Function Overloads]]"
  - "[[Operator Overload Selection Guide]]"
  - "[[Member Functions]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Member Function Overloads

Overloading an operator as a **member function** makes the operator a method of the class. The **left operand becomes the implicit `*this` object**, and all remaining operands are explicit parameters.

```cpp
class Cents
{
public:
    // Left operand (the Cents object) is *this; right operand is 'value'
    Cents operator+(int value) const
    {
        return Cents{ m_cents + value };
    }
private:
    int m_cents{};
};
```

Compared to a non-member overload, the left parameter is dropped from the explicit parameter list because it becomes `this`.

## Operators that *must* be member functions

The language requires these operators to be overloaded as member functions:

| Operator | Symbol |
|---|---|
| Assignment | `=` |
| Subscript | `[]` |
| Function call | `()` |
| Member selection (pointer) | `->` |

## Operators that *cannot* be member functions

`operator<<` and `operator>>` cannot be member functions because the left operand is `std::ostream`/`std::istream` — a type you do not own. Use a [[Friend Function Overloads|friend]] or [[Normal Function Overloads|normal]] function instead.

## Unary operators

Unary operators (e.g. `operator-`, `operator!`) should be implemented as member functions, since there is no left operand to worry about.

See [[Operator Overload Selection Guide]] for the complete decision rules.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Operator Overloading → Member Function Overloads
