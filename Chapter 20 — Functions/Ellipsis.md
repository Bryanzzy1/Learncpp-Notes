---
tags:
  - cpp/functions
  - concept
  - syntax
  - best-practice
aliases:
  - variadic functions
  - va_list
  - va_start
  - va_arg
  - va_end
  - variadic arguments
  - "..."
up: "[[Chapter 20 — Functions]]"
related:
  - "[[Functions]]"
  - "[[Function Templates]]"
  - "[[Undefined Behavior]]"
  - "[[Chapter 20 — Functions]]"
---

# Ellipsis

**Ellipsis** (`...`) allow a function to accept a variable number of arguments of any type. They are inherited from C and are generally unsafe — prefer [[Function Templates]] or `std::initializer_list` in modern C++.

## Syntax

The ellipsis must be the **last** parameter:

```cpp
#include <cstdarg>

double findAverage(int count, ...)
{
    int sum{ 0 };

    std::va_list list;
    va_start(list, count);   // initialize; second arg is the last named parameter

    for (int arg{ 0 }; arg < count; ++arg)
        sum += va_arg(list, int); // extract next argument as int

    va_end(list);            // clean up

    return static_cast<double>(sum) / count;
}
```

## Why ellipsis are dangerous

### 1. Type checking is suspended

The compiler performs no type checking on ellipsis arguments. Passing a `double` where `int` is expected is [[Undefined Behavior]] — the compiler won't warn you.

### 2. The function doesn't know argument count

There is no built-in way to determine how many arguments were passed. Three common workarounds:

| Method | How | Risk |
|---|---|---|
| **Length parameter** | Pass count explicitly (as above) | Caller can lie or forget |
| **Sentinel value** | End list with a special terminator (`-1`, `nullptr`) | Easy to forget the sentinel |
| **Decoder string** | Pass a format string encoding the types | Complex; still no type safety |

Decoder string example:

```cpp
va_start(list, decoder);
for (auto codetype : decoder)
{
    switch (codetype)
    {
    case 'i': sum += va_arg(list, int);    break;
    case 'd': sum += va_arg(list, double); break;
    }
}
```

## Best practices

- If you must use ellipsis, pass all values as the **same type** — mixing types compounds the safety problems.
- A length parameter or decoder string is safer than a sentinel (sentinels are easy to omit).
- Prefer [[Function Templates]] for type-safe variadic behavior, or `std::initializer_list` for homogeneous lists.

> Full coverage: [[Chapter 20 — Functions]] → Ellipsis
