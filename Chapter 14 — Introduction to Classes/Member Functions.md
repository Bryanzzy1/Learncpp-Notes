---
tags:
  - cpp/classes
  - cpp/functions
  - concept
  - syntax
  - subnode
aliases:
  - member function
  - implicit object
  - member function overloading
up: "[[Classes]]"
related:
  - "[[Classes]]"
  - "[[Const Member Functions]]"
  - "[[Member Access]]"
  - "[[Structs]]"
  - "[[Functions]]"
  - "[[Function Overloading]]"
  - "[[Pass by Value]]"
---

# Member Functions

A **member function** is a function declared inside a class type definition. It operates on an instance of that class.

## Declaration and definition

Member functions **must be declared inside** the class type definition, but may be **defined either inside or outside** the class:

```cpp
class Foo
{
public:
    int getValue();       // declaration inside class
};

int Foo::getValue() { return m_data; } // definition outside class
```

## The implicit object

When a member function is called on an object, that object is **implicitly passed** to the function. It is called the **implicit object**. Inside the member function, data members are accessed directly because they belong to the implicit object.

## Members can be defined in any order

Inside a class definition, the compiler performs a special trick: **member functions defined inside the class body are implicitly forward declared**, then their definitions are moved after the closing `}`. This means member functions can refer to members declared later in the class:

```cpp
// What the compiler effectively compiles:
struct Foo
{
    int z(); // forward declared
    int x(); // forward declared
    int y(); // forward declared
    int m_data{};
};

int Foo::z() { return m_data; }
int Foo::x() { return y(); }
int Foo::y() { return 5; }
```

Data members are still **initialized in declaration order**, regardless of their order in member initializer lists.

## Member function overloading

Member functions support [[Function Overloading|overloading]] — multiple member functions can share the same name as long as their parameter lists differ.

## Structs vs classes

Member functions work with both `struct` and `class`. However, **structs should avoid defining constructor member functions** — doing so makes them non-aggregates and disables aggregate initialization.

> Full coverage: [[Chapter 14 — Introduction to Classes]] → Member Functions
