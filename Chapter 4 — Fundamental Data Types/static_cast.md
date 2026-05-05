---
tags:
  - cpp/casting
  - cpp/types
  - concept
  - syntax
aliases:
  - type casting
  - explicit type conversion
  - implicit type conversion
up: "[[Chapter 4 — Fundamental Data Types|Chapter 4 — Fundamental Data Types]]"
related:
  - "[[Initialization]]"
  - "[[Memory Model]]"
  - "[[size_t]]"
  - "[[Implicit Conversion]]"
---

# static_cast

The preferred operator for explicit type conversions in C++.

```cpp
static_cast<new_type>(expression)

static_cast<int>(5.5)               // double → int, truncates
static_cast<int>(name.length())     // size_t → int, avoids signed/unsigned warning
```

- Also invoked via **direct-initialization**: `int c(static_cast<int>(x))`
- If the value cannot be represented in the destination type:
  - **Unsigned** destination → value is modulo wrapped
  - **Signed** destination → implementation-defined before C++20; modulo wrapped as of C++20
- **[[Implicit Conversion|Implicit type conversion]]** happens without an explicit cast — the compiler converts on your behalf, which can silently lose precision

> Full coverage: [[Chapter 4 — Fundamental Data Types|Chapter 4 — Fundamental Data Types]] → Type Casting
