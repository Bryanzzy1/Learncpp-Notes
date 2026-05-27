---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - syntax
  - best-practice
aliases:
  - destructor
  - ~ClassName
  - RAII
  - cleanup
up: "[[Chapter 15 — More on Classes]]"
related:
  - "[[Constructors]]"
  - "[[Classes]]"
  - "[[Static Local Variables]]"
  - "[[Global Variables]]"
  - "[[Memory Model]]"
---

# Destructors

A **destructor** is a special member function automatically called when an object of the class is destroyed. Its job is to release resources or perform any necessary cleanup before the object's lifetime ends.

## Rules

1. The destructor name is `~ClassName` (tilde + class name, exactly).
2. It takes **no arguments**.
3. It has **no return type**.
4. A class can have **only one** destructor.

```cpp
class Simple
{
public:
    ~Simple()
    {
        std::cout << "Destructing Simple " << m_id << '\n';
    }
private:
    int m_id{};
};
```

## Implicit destructor

If a non-aggregate class type has no user-declared destructor, the compiler generates one with an empty body. This is fine for classes that don't manage external resources.

## Object lifetime and destructor calls

Destructors are called in the reverse order of construction:

- Local variables: when they go out of scope.
- Objects with static duration ([[Global Variables]], [[Static Local Variables]]): when the program ends.
- Dynamically allocated objects: when `delete` is called.

## When destructors are NOT called

- **`std::exit()`** terminates the program immediately — local variable destructors are not called.
- **Unhandled exceptions** may terminate without unwinding the stack, so destructors may not run.

This is why resource-owning classes should use RAII (Resource Acquisition Is Initialization): tie resource lifetime to object lifetime so the destructor handles cleanup automatically.

## Rule of three / rule of five

If a class needs a user-defined destructor, it almost certainly also needs a user-defined [[Copy Constructor|copy constructor]] and copy assignment operator (rule of three). In C++11+, add move constructor and move assignment (rule of five).

> Full coverage: [[Chapter 15 — More on Classes]] → Destructors
