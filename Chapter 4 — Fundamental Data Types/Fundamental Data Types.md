---
tags:
  - cpp/types
  - concept
aliases:
  - fundamental types
  - built-in types
  - incomplete type
  - void
  - integral types
up: "[[Chapter 4 — Fundamental Data Types]]"
related:
  - "[[Integers]]"
  - "[[Floating Point]]"
  - "[[Boolean]]"
  - "[[Char]]"
  - "[[size_t]]"
  - "[[Memory Model]]"
  - "[[static_cast]]"
---

# Fundamental Data Types

C++ built-in types, organized by category:

| Category | Types |
|----------|-------|
| Integer | `short`, `int`, `long`, `long long` (and `unsigned` variants) |
| Boolean | `bool` |
| Character | `char`, `wchar_t`, `char8_t`, `char16_t`, `char32_t` |
| Floating point | `float`, `double`, `long double` |
| Null pointer | `std::nullptr_t` |
| Void | `void` |

**Key rules:**
- The C++ standard defines **minimum sizes**, not exact sizes — use fixed-width integers when exact size matters.
- **Integral types** = `bool` + all `char` types + all integer types.
- An object must occupy at least 1 byte so that each object has a unique memory address.
- **Incomplete type**: declared but not yet defined. `void` is the canonical example — you cannot create a `void` object or call `sizeof(void)`.

See also: [[Integers]], [[Floating Point]], [[Boolean]], [[Char]], [[size_t]]

> Full coverage: [[Chapter 4 — Fundamental Data Types]] → Fundamental data types
