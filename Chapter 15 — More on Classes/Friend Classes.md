---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - syntax
  - subnode
aliases:
  - friend class
  - friend member function
  - class friendship
up: "[[Friend Functions]]"
related:
  - "[[Friend Functions]]"
  - "[[Classes]]"
  - "[[Member Access]]"
  - "[[Data Hiding]]"
---

# Friend Classes

A **friend class** is a class that has been granted full access to the `private` and `protected` members of another class.

```cpp
class Storage
{
private:
    int m_data { 0 };

    friend class Display; // Display can access Storage's private members
};

class Display
{
public:
    void show(const Storage& s)
    {
        std::cout << s.m_data; // OK — Display is a friend of Storage
    }
};
```

## Friendship properties

| Property | Rule |
|----------|------|
| Transitivity | **Not transitive.** If A friends B and B friends C, A is not a friend of C. |
| Inheritance | **Not inherited.** If A friends B, classes derived from B are not friends of A. |
| Symmetry | **Not symmetric.** If A friends B, B is not automatically a friend of A. |

## Friend member functions

You can grant access to a **specific member function** of another class rather than the whole class:

```cpp
class Storage
{
    friend void Display::displayStorage(const Storage& storage);
};
```

### Declaration order requirement

For a specific member function to be friended, the **full definition of the member function's class** must be visible before the friend declaration. If there is a circular dependency, use a forward declaration of the class and define the member function after both classes are defined:

```cpp
class Storage; // forward declaration

class Display
{
public:
    void displayStorage(const Storage& storage); // declaration only — full body comes later
};

class Storage
{
    friend void Display::displayStorage(const Storage& storage); // now valid
private:
    int m_data { 0 };
};

// Now define the member function body
void Display::displayStorage(const Storage& storage)
{
    std::cout << storage.m_data;
}
```

> Full coverage: [[Chapter 15 — More on Classes]] → Friend Classes
