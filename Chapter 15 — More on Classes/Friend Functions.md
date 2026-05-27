---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - syntax
  - best-practice
aliases:
  - friend function
  - friend declaration
  - friend keyword
  - non-member friend
up: "[[Chapter 15 — More on Classes]]"
related:
  - "[[Friend Classes]]"
  - "[[Classes]]"
  - "[[Member Access]]"
  - "[[Data Hiding]]"
  - "[[Member Functions]]"
---

# Friend Functions

A **friend function** is a function that has been granted full access to the `private` and `protected` members of a class, even though it is not a member of that class.

## Declaring a friend

Use the `friend` keyword inside the class definition to grant access:

```cpp
class Accumulator
{
private:
    int m_value { 0 };

public:
    friend void print(const Accumulator& accumulator); // friend declaration
};

// Non-member function — has access to private members
void print(const Accumulator& accumulator)
{
    std::cout << accumulator.m_value; // OK because print is a friend
}
```

The friend declaration can appear under any access specifier — placement under `public`, `private`, or `protected` does not change the effect.

## Non-member nature

Because `print()` is a non-member function, it has no implicit object. The `Accumulator` object must be passed **explicitly** as a parameter.

## Defining a friend inside the class body

A friend function can be defined inside the class body — it is still a non-member function:

```cpp
class Accumulator
{
    friend void print(const Accumulator& a)
    {
        std::cout << a.m_value;
    }
    // ...
};
```

## Best practices

- **Prefer non-friend functions** when the public interface is sufficient. Friends break [[Data Hiding|encapsulation]] by coupling the function to the class's private implementation.
- When a friend function is necessary, **prefer using the class interface** over direct private access wherever possible.

## Subnodes

- [[Friend Classes]] — extending friendship to entire classes and to specific member functions

> Full coverage: [[Chapter 15 — More on Classes]] → Friend Functions
