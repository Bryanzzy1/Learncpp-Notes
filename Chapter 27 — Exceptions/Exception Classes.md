---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - cpp/classes
  - concept
  - syntax
  - best-practice
aliases:
  - exception class
  - exception object
  - user-defined exception
  - ArrayException
  - exception design
up: "[[Chapter 27 — Exceptions]]"
related:
  - "[[std-exception|std::exception]]"
  - "[[Basic Exception Handling]]"
  - "[[Rethrowing Exceptions]]"
  - "[[Constructors]]"
  - "[[Basic Inheritance]]"
  - "[[Object Slicing]]"
  - "[[Assertions]]"
  - "[[Chapter 27 — Exceptions]]"
---

# Exception Classes

An **exception class** is a regular C++ class designed specifically to be thrown as an exception. Using classes (rather than primitives like `int` or `const char*`) lets you carry structured error information and take advantage of [[Basic Inheritance|inheritance]] for exception hierarchies.

## Defining an exception class

```cpp
class ArrayException
{
private:
    std::string m_error;

public:
    ArrayException(std::string_view error)
        : m_error{ error }
    {
    }

    const std::string& getError() const { return m_error; }
};

// Usage
throw ArrayException("Index out of bounds");
```

## Exceptions in member functions

Class member functions can throw instead of using [[Assertions|assert()]] for conditions that callers must handle:

```cpp
T& operator[](int index)
{
    if (index < 0 || index >= m_size)
        throw ArrayException("Index out of bounds");
    return m_data[index];
}
```

## Exceptions in constructors

[[Constructors|Constructors]] cannot return error codes. Throwing from a constructor is the standard way to signal construction failure:

```cpp
Widget(int size)
{
    if (size <= 0)
        throw std::invalid_argument("size must be positive");
    m_data = new int[size];
}
```

When a constructor throws, any **already-constructed members** have their destructors called automatically. Resource acquisition inside members (RAII) therefore cleans up safely even on partial construction. However, the destructor of the partially-constructed object itself is **not** called — so avoid raw resource ownership in the constructor body itself; prefer RAII member wrappers.

## Catching by reference (not value)

- **Fundamental types** (`int`, `const char*`): catch by value — they are cheap to copy.
- **Class exceptions**: catch by `const` reference — avoids expensive copying and prevents [[Object Slicing]] if the thrown type is derived.

```cpp
catch (const ArrayException& e)   // ✓ — by const reference
{
    std::cerr << e.getError() << '\n';
}
```

## Inheritance ordering

When using exception hierarchies, place the most-derived handler before base handlers. C++ matches the **first** compatible catch:

```cpp
catch (const Derived& e) { /* must come first */ }
catch (const Base& e)    { /* catches Base and anything not caught above */ }
```

Reversing the order means the `Base` handler catches everything, and the `Derived` handler is unreachable.

## Exception object lifetime

When an exception is thrown, the compiler copies the exception object to unspecified storage reserved for exception handling (outside the call stack). Therefore:
- Exception objects must be **copyable**.
- Exception objects must **not** hold pointers or references to stack-allocated objects (those frames are unwound before the catch runs).

## Subnode

- [[std-exception|std::exception]] — the standard library's exception base class and common derived types

> Full coverage: [[Chapter 27 — Exceptions]] → Exception Classes
