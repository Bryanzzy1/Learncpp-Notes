---
tags:
  - cpp/types
  - cpp/boolean
  - concept
  - syntax
aliases:
  - bool
  - boolean
  - boolalpha
up: "[[Chapter 4 — Fundamental Data Types]]"
related:
  - "[[IO]]"
  - "[[Initialization]]"
  - "[[Fundamental Data Types]]"
  - "[[Output Manipulators]]"
---

# Boolean

`bool` stores exactly one of two values: `true` (stored as 1) or `false` (stored as 0).

```cpp
bool b1 { true };
bool b2 { false };
bool b3 {};          // value-initialized to false
```

**Output:**
- `std::cout` prints `1` / `0` by default
- Use [[Output Manipulators|`std::boolalpha`]] to print `true` / `false`:

```cpp
std::cout << std::boolalpha << b1;  // prints "true"
```

**Input:**
- `std::cin` accepts only `0` or `1` for `bool` variables by default
- Enable `std::boolalpha` on `cin` to accept `"true"` / `"false"`

> Full coverage: [[Chapter 4 — Fundamental Data Types]] → Boolean
