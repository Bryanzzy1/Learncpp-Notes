---
tags:
  - cpp/classes
  - cpp/templates
  - concept
  - syntax
  - best-practice
aliases:
  - class template member function
  - injected class name
  - template member function outside class
up: "[[Chapter 15 — More on Classes]]"
related:
  - "[[Class Templates]]"
  - "[[Function Templates]]"
  - "[[CTAD]]"
  - "[[One Definition Rule]]"
  - "[[Header Files]]"
  - "[[Inline Functions and Variables]]"
  - "[[Constructors]]"
---

# Class Template Member Functions

When a class template has member functions, those functions can be defined **inside** or **outside** the class body, with different syntax requirements for each.

## Defined inside the class body

When a member function is defined inside the class definition, the class's template parameter declaration automatically applies — no extra `template<>` line is needed:

```cpp
template <typename T>
class Pair
{
private:
    T m_first{};
    T m_second{};

public:
    Pair(const T& first, const T& second)
        : m_first{ first }, m_second{ second }
    {
    }

    bool isEqual(const Pair<T>& pair);
};
```

## Defined outside the class body

Each out-of-class member function definition must be prefixed with its own `template` parameter declaration, and the class name must be written as the full template specialization (`Pair<T>`):

```cpp
template <typename T>
bool Pair<T>::isEqual(const Pair<T>& pair)
{
    return m_first == pair.m_first && m_second == pair.m_second;
}
```

Keep out-of-class definitions **immediately below the class definition** in the same file — the compiler needs both the class definition and the function definition together to instantiate the template.

## Injected class name

Within the scope of a class template, the unqualified class name (e.g. `Pair` without `<T>`) is the **injected class name** — it serves as shorthand for the fully templated name `Pair<T>`. This is why member functions defined *inside* the class body can refer to `Pair` without `<T>`.

## CTAD and constructors

Non-aggregate class templates don't need explicit [[CTAD|deduction guides]] when they have constructors. A matching constructor provides enough information for the compiler to deduce template parameters automatically:

```cpp
Pair p { 1, 2 }; // T deduced as int from the constructor
```

## Header placement

Like all template definitions, class template member functions must be visible in every [[Translation Unit]] that instantiates them. Because member functions of class templates are **implicitly inline**, including their definitions in multiple translation units via a [[Header Files|header file]] does not violate the [[One Definition Rule]].

> Full coverage: [[Chapter 15 — More on Classes]] → Class Template Member Functions
