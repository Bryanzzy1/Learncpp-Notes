---
tags:
  - cpp/namespaces
  - concept
  - syntax
  - best-practice
aliases:
  - unnamed namespace
  - anonymous namespace
  - inline namespace
up: "[[Chapter 7 — Scope, duration, and linkage]]"
related:
  - "[[User-Defined Namespaces]]"
  - "[[Internal Linkage]]"
  - "[[Using Declarations and Directives]]"
---

# Unnamed and Inline Namespaces

## Unnamed (anonymous) namespaces

An **unnamed namespace** is a namespace declared without a name:

```cpp
namespace // unnamed namespace
{
    void doSomething() // can only be accessed in this file
    {
        std::cout << "v1\n";
    }
}

int main()
{
    doSomething(); // no prefix needed
    return 0;
}
```

- All identifiers inside an unnamed namespace have **internal linkage** automatically — they cannot be seen outside the translation unit.
- For functions, this is equivalent to declaring them `static`.
- **Prefer unnamed namespaces** when you have content to keep local to a translation unit — they work for a wider range of identifiers (e.g., type definitions) than `static`.
- **Avoid unnamed namespaces in header files.**

## Inline namespaces

An **inline namespace** is used to version content while keeping the default (current) version accessible without a prefix:

```cpp
#include <iostream>

inline namespace V1 // default version
{
    void doSomething() { std::cout << "V1\n"; }
}

namespace V2
{
    void doSomething() { std::cout << "V2\n"; }
}

int main()
{
    V1::doSomething(); // explicit V1
    V2::doSomething(); // explicit V2
    doSomething();     // calls V1 (the inline version)
    return 0;
}
```

## Combining both

A namespace can be both inline and unnamed. However, it is generally better to nest an anonymous namespace inside an inline namespace for clarity.

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Unnamed and Inline Namespaces
