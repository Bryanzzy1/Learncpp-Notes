---
tags:
  - cpp/io
  - concept
  - syntax
  - subnode
aliases:
  - insertion operator
  - operator<<
  - output flags
  - stream flags
  - setf
  - unsetf
  - format group
  - ios formatting
  - output manipulators
up: "[[IO Streams]]"
related:
  - "[[IO Streams]]"
  - "[[istream]]"
  - "[[File IO|File I/O]]"
  - "[[IO]]"
  - "[[Chapter 28 — Input and Output]]"
---

# ostream

`ostream` is the C++ class for output streams. `std::cout` and `std::cerr` are `ostream` objects; `ofstream` (for files) is a derived class. The primary operation is the **insertion operator `<<`**, which writes formatted data into the stream.

## Two formatting mechanisms

`ostream` (via its base class `ios`) provides two parallel ways to control output formatting:

| Mechanism | What it is | How to apply |
|---|---|---|
| **Flags** | Boolean state variables stored on the stream | `setf()` / `unsetf()` |
| **Manipulators** | Objects inserted inline with `<<` or `>>` | Place in the stream expression |

Both affect subsequent output until changed again (flags are sticky; most manipulators are sticky too, with `setw` being the notable exception).

## Flags

Each stream maintains a set of format flags (bitmask values on the `ios` base class). To switch a flag on:

```cpp
std::cout.setf(std::ios::showpos);   // show '+' before positive numbers
std::cout << 42 << '\n';             // prints "+42"
```

To turn a flag off:

```cpp
std::cout.unsetf(std::ios::showpos);
```

## Format groups

A **format group** is a set of flags that are mutually exclusive — only one can be active at a time. Examples:

- **`basefield`** — `dec`, `oct`, `hex` (which base to use for integers)
- **`floatfield`** — `fixed`, `scientific` (how to format floating-point numbers)
- **`adjustfield`** — `left`, `right`, `internal` (how to pad output)

When setting a flag that belongs to a format group, you should clear the whole group first using the two-argument form of `setf`:

```cpp
std::cout.setf(std::ios::hex, std::ios::basefield);   // clear basefield, then set hex
std::cout << 255 << '\n';   // prints "ff"
```

## Manipulators

Manipulators (from `<iomanip>` and `<ios>`) are the more readable alternative to `setf`/`unsetf`. They are placed directly in the stream expression with `<<`:

```cpp
#include <iomanip>
std::cout << std::hex << 255 << '\n';        // "ff"
std::cout << std::setw(10) << 42 << '\n';   // right-aligned in 10 chars
std::cout << std::fixed << std::setprecision(2) << 3.14159 << '\n'; // "3.14"
```

For the basic manipulators used in everyday code (`std::endl`, `std::boolalpha`, etc.) see [[IO]].

> Full coverage: [[Chapter 28 — Input and Output]] → ostream
