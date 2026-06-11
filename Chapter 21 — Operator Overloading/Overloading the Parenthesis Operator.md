---
tags:
  - cpp/operators
  - cpp/classes
  - cpp/functions
  - concept
  - syntax
aliases:
  - operator() overload
  - function call operator
  - functor
  - function object
up: "[[Chapter 21 — Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Member Function Overloads]]"
  - "[[Lambdas]]"
  - "[[Function Pointers]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Overloading the Parenthesis Operator

`operator()` must be overloaded as a **member function** (the language requires it). Unlike other operators, it allows you to vary **both the number and type of parameters** freely.

```cpp
class Matrix
{
public:
    double operator()(int row, int col) const
    {
        return m_data[row][col];
    }
private:
    double m_data[4][4]{};
};

Matrix m{};
double val{ m(1, 2) }; // calls operator()(1, 2)
```

## Functors (function objects)

The primary use of `operator()` is to create **functors** — class instances that behave like functions. The advantage over a plain function is that a functor can **store state** in member variables:

```cpp
class Adder
{
public:
    Adder(int base) : m_base{ base } {}
    int operator()(int x) const { return m_base + x; }
private:
    int m_base;
};

Adder add5{ 5 };
std::cout << add5(3); // prints 8
```

A functor passed to a standard library algorithm carries its state with it — something a plain [[Function Pointers|function pointer]] cannot do without global variables.

## Relationship to lambdas

[[Lambdas]] are the modern, syntactically concise way to create functors. The compiler implements each lambda as a compiler-generated class with an `operator()` overload. Functors written by hand are useful when you need a named, reusable callable with complex state that a lambda would make unwieldy.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Overloading the Parenthesis Operator
