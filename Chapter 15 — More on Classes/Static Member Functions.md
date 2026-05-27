---
tags:
  - cpp/classes
  - cpp/static
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - static member function
  - static method
  - static class function
up: "[[Static Member Variables]]"
related:
  - "[[Static Member Variables]]"
  - "[[this Pointer]]"
  - "[[Classes]]"
  - "[[Global Variables]]"
  - "[[User-Defined Namespaces]]"
  - "[[Member Functions]]"
---

# Static Member Functions

A **static member function** is a member function that is not associated with any particular object of the class. Because it has no implicit object, it has **no `this` pointer**.

```cpp
class Something
{
private:
    static inline int s_value { 1 };

public:
    static int getValue() { return s_value; } // static member function
};

int x = Something::getValue(); // called via class name, not an object
```

## Access rules

- Can directly access other **static** members (variables or functions).
- **Cannot** access non-static data members or non-static member functions (there is no object to access them through).

## Calling static member functions

Prefer calling through the class name and scope resolution operator:

```cpp
Something::getValue(); // preferred
Something s{};
s.getValue();          // works but misleading — avoid
```

## Defined outside the class

Static member functions can be defined outside the class body. The `static` keyword is **only** used in the declaration inside the class, not in the out-of-class definition:

```cpp
// inside class:
static int getValue();

// outside class (no 'static' keyword):
int Something::getValue() { return s_value; }
```

## Static class vs namespace

| Use a static class when | Use a namespace when |
|------------------------|---------------------|
| You need static data members | No shared state is needed |
| Access controls (private/public) matter | Access controls are not needed |

**C++ does not support static constructors.** If you need to initialize static data at program start, use inline static member initialization or a static local variable trick.

If you find yourself writing a class with only static members, consider whether a global instance of a normal class (with [[Global Variables|static duration]]) would be cleaner — local instances can still be created when needed.

> Full coverage: [[Chapter 15 — More on Classes]] → Static Member Functions
