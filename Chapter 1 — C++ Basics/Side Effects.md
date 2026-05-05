---
tags:
  - cpp/basics
  - concept
  - subnode
aliases:
  - side effect
  - operator chaining
  - observable effect
up: "[[IO]]"
related:
  - "[[IO]]"
  - "[[Initialization]]"
---

# Side Effects

In C++, a **side effect** is an observable effect of an expression beyond producing a return value — modifying a variable, writing to the console, reading from input, etc.

```cpp
x = 5;              // side effect: x is modified
std::cout << 5;     // side effect: console output occurs
```

## Operator Chaining

`operator=` and `operator<<` both **return their left operand**, which enables chaining:

```cpp
// operator= returns the left operand
x = y = 5;          // evaluates as x = (y = 5)
                    // y = 5 assigns 5 to y and returns y
                    // x = y then assigns y's value to x

// operator<< returns std::cout
std::cout << a << b;  // evaluates as (std::cout << a) << b
                      // first call returns std::cout, which is used by the second
```

This is why multi-step assignments and chained output both work without intermediate variables.

> Full coverage: [[Chapter 1 — C++ Basics]] → I/O
