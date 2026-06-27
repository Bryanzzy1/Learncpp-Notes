---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - cpp/stdlib
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - std::exception
  - std::runtime_error
  - std::logic_error
  - std::bad_cast
  - exception hierarchy
  - what()
up: "[[Exception Classes]]"
related:
  - "[[Exception Classes]]"
  - "[[Basic Inheritance]]"
  - "[[Dynamic Casting]]"
  - "[[Chapter 27 — Exceptions]]"
---

# std::exception

`std::exception` is a small base class provided by `<exception>` that serves as the root of C++'s standard exception hierarchy. All exceptions thrown by the standard library derive from it.

## Interface

```cpp
class std::exception
{
public:
    virtual const char* what() const noexcept; // returns a human-readable description
    virtual ~std::exception();
};
```

`what()` is the only meaningful method — it returns a C-style string describing the error.

## Catching all standard exceptions

Because of [[Basic Inheritance|inheritance]], catching `const std::exception&` handles any standard library exception:

```cpp
try
{
    std::string s;
    s.resize(std::numeric_limits<std::size_t>::max()); // triggers std::length_error
}
catch (const std::exception& e)
{
    std::cerr << "Standard exception: " << e.what() << '\n';
}
```

## Common standard exception types

| Class | Header | Typical cause |
|---|---|---|
| `std::runtime_error` | `<stdexcept>` | Errors detectable only at runtime |
| `std::logic_error` | `<stdexcept>` | Errors in program logic (precondition violations) |
| `std::invalid_argument` | `<stdexcept>` | Invalid argument passed to a function |
| `std::out_of_range` | `<stdexcept>` | Index/range violation (e.g. `std::vector::at`) |
| `std::bad_alloc` | `<new>` | `new` fails to allocate memory |
| `std::bad_cast` | `<typeinfo>` | Failed `dynamic_cast` on a reference (see [[Dynamic Casting]]) |

## Using standard exceptions directly

`std::runtime_error` is a common choice for general errors because its constructor accepts a message:

```cpp
throw std::runtime_error("Bad things happened");
```

It accepts either a `const char*` or a `const std::string&`.

## Inheriting from std::exception

Derive your own exception classes from `std::exception` (or one of its subclasses) so callers can catch them with a single `const std::exception&` handler:

```cpp
class MyException : public std::runtime_error
{
public:
    MyException(const std::string& msg)
        : std::runtime_error{ msg }
    {
    }
};
```

> Full coverage: [[Chapter 27 — Exceptions]] → Exception Classes → std::exception
