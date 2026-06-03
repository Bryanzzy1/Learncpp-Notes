---
tags:
  - cpp/utilities
  - cpp/references
  - concept
  - syntax
aliases:
  - std::reference_wrapper
  - reference_wrapper
  - std::ref
  - std::cref
  - reseat reference
up: "[[Chapter 17 — Dynamic memory allocation]]"
related:
  - "[[Lvalue References]]"
  - "[[Function Templates]]"
  - "[[Containers and Arrays]]"
---

# std::reference_wrapper

`std::reference_wrapper<T>` is a standard library class template in `<functional>`. It behaves like a **modifiable lvalue reference** — but unlike a raw [[Lvalue References|lvalue reference]], it is an object and can be reseated (changed to refer to a different object).

## Key behaviors

| Operation | Behavior |
|---|---|
| `operator=` | **Reseats** the wrapper — changes which object is referenced |
| Implicit conversion | Converts to `T&` automatically in most contexts |
| `get()` | Explicitly returns `T&` — useful when assigning through the wrapper |

```cpp
int a { 1 }, b { 2 };
std::reference_wrapper<int> ref { a };

ref = b;          // reseats: ref now refers to b (not a copy of b)
ref.get() = 99;   // modifies b through the wrapper
```

## Typical use case

The primary motivation is storing references in containers. Containers require their element type to be copyable/assignable; raw references are not. `std::reference_wrapper` satisfies those requirements while preserving reference semantics:

```cpp
std::vector<std::reference_wrapper<int>> refs { a, b };
refs[0].get() = 10; // modifies a
```

This pattern works well with [[Function Templates|function templates]] that need to operate on a heterogeneous collection without copying the objects.

> Full coverage: [[Chapter 17 — Dynamic memory allocation]] → std::reference_wrapper
