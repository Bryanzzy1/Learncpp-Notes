---
tags:
  - cpp/types
  - cpp/memory
  - concept
aliases:
  - std::size_t
  - size type
  - sizeof
up: "[[Chapter 4 — Fundamental Data Types|Chapter 4 — Fundamental Data Types]]"
related:
  - "[[Memory Model]]"
  - "[[static_cast]]"
---

# size_t (std::size_t)

An unsigned integer type used to represent sizes, counts, and memory offsets. The compiler decides its exact underlying type (`unsigned int`, `unsigned long`, or `unsigned long long`).

**Returned by:**
- `sizeof(type)` or `sizeof(variable)` — size in bytes of a type or object
- `std::string::length()` and similar size functions

```cpp
sizeof(int)                              // → std::size_t (e.g., 4)
std::string name { "Alex" };
std::size_t len = name.length();         // unsigned
int         len = name.length();         // warning: signed/unsigned mismatch

int safeLen { static_cast<int>(name.length()) };  // explicit [[static_cast]]
```

**Constraints:**
- An object's byte size cannot exceed the maximum value of `std::size_t`
- Mixing `size_t` with signed integers triggers compiler warnings — use [[static_cast]] to convert safely

> Full coverage: [[Chapter 4 — Fundamental Data Types|Chapter 4 — Fundamental Data Types]] → sizeof operator
