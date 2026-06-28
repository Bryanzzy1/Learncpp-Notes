---
tags:
  - cpp/io
  - concept
  - syntax
  - subnode
aliases:
  - extraction operator
  - operator>>
  - setw
  - get()
  - istream input
  - stream input
up: "[[IO Streams]]"
related:
  - "[[IO Streams]]"
  - "[[ostream]]"
  - "[[File IO|File I/O]]"
  - "[[IO]]"
  - "[[getline]]"
  - "[[Chapter 28 — Input and Output]]"
---

# istream

`istream` is the C++ class for input streams. `std::cin` is an `istream` object; `ifstream` (for files) is a derived class. The primary operation is the **extraction operator `>>`**, which reads formatted data out of the stream.

## Extraction operator

```cpp
int x;
std::cin >> x;   // reads an integer from standard input
```

The `>>` operator:
- **Skips leading whitespace** (spaces, tabs, newlines) before reading.
- Stops at the next whitespace after the value.
- Returns the stream so extractions can be chained: `std::cin >> a >> b`.

This is the same `std::cin` introduced in [[IO]], but the underlying class is `istream`.

## Manipulators

A **manipulator** is an object placed in a stream expression (with `>>` or `<<`) that modifies how the stream processes data. For input, the most commonly used manipulator is `std::setw`.

### setw — limit input width

`std::setw` (from `<iomanip>`) limits the number of characters read into a buffer, preventing overflow:

```cpp
#include <iomanip>

char buf[10]{};
std::cin >> std::setw(10) >> buf;   // reads at most 9 chars + null terminator
```

`setw` applies only to the next extraction — it is not sticky.

## get() — single character extraction

`get()` reads one character from the stream, **including whitespace** (unlike `>>`):

```cpp
char ch{};
std::cin.get(ch);   // reads exactly one character, no whitespace skip
```

Use `get()` when you need precise character-by-character control over the input.

## getline() — full-line extraction

`getline()` reads characters until a delimiter (default: `\n`) is found. The delimiter is consumed but **not stored**:

```cpp
std::string line;
std::getline(std::cin, line);   // reads the entire line including spaces
```

This is the same behavior as [[getline|`std::getline`]] covered in Chapter 1. The key distinction from `>>`:

| Method | Whitespace | Stops at |
|---|---|---|
| `operator>>` | Skips leading whitespace | Next whitespace |
| `get()` | Does not skip | Reads one char |
| `getline()` | Does not skip | Delimiter (`\n`) |

> Full coverage: [[Chapter 28 — Input and Output]] → istream
