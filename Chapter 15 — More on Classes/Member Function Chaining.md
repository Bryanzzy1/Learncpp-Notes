---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - syntax
  - subnode
aliases:
  - method chaining
  - fluent interface
  - returning *this
up: "[[this Pointer]]"
related:
  - "[[this Pointer]]"
  - "[[Member Functions]]"
  - "[[Const Member Functions]]"
  - "[[Classes]]"
---

# Member Function Chaining

**Member function chaining** (also called a fluent interface) is a pattern where member functions return a reference to the implicit object (`*this`), allowing multiple calls to be chained on a single expression.

```cpp
class Calc
{
private:
    int m_value{};

public:
    Calc& add(int value)  { m_value += value; return *this; }
    Calc& sub(int value)  { m_value -= value; return *this; }
    Calc& mult(int value) { m_value *= value; return *this; }

    int getValue() const { return m_value; }
};

Calc calc{};
calc.add(5).sub(3).mult(4); // chains: result is (5-3)*4 = 8
```

Each function returns `Calc&` — a reference to the same object — so the next call in the chain operates on the same `Calc` instance.

## Resetting an object to its default state

Dereferencing `this` and assigning from a value-initialized temporary is an idiom for resetting an object to its default state:

```cpp
void reset()
{
    *this = {}; // value-initializes a new Calc and overwrites the implicit object
}
```

This works because `*this` is an lvalue reference to the implicit object, and the assignment copies the new temporary's state into it.

## Const correctness

For [[Const Member Functions|const member functions]], `this` is a `const ClassName* const`, so `*this` is a `const` object — only `const` member functions can be called on it, and the return type must be `const ClassName&` (not `ClassName&`) to preserve constness.

> Full coverage: [[Chapter 15 — More on Classes]] → Member Function Chaining
