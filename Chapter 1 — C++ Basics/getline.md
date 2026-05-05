---
tags:
  - cpp/io
  - cpp/strings
  - concept
  - syntax
  - subnode
aliases:
  - std::getline
  - getline
  - std::ws
  - whitespace extraction
up: "[[IO]]"
related:
  - "[[IO]]"
  - "[[Strings]]"
  - "[[Initialization]]"
---

# std::getline

`std::getline()` reads an entire line from a stream, including spaces, until it encounters a newline (`\n`). The newline is consumed but not stored.

```cpp
std::string name;
std::getline(std::cin, name);              // reads "John Doe" correctly
std::cin >> name;                          // only reads "John" — stops at space
```

## std::ws — Discard Leading Whitespace

When mixing `operator>>` and `std::getline()` in the same program, a leftover newline in the buffer from the previous `>>` extraction will be consumed as an empty line by `getline`. Fix this with `std::ws`:

```cpp
int age;
std::cin >> age;                              // leaves '\n' in buffer
std::getline(std::cin >> std::ws, name);      // std::ws eats the '\n' first
```

`std::ws` discards all leading whitespace (spaces, tabs, newlines) from the stream before the next extraction.

## When to Use

| Situation | Use |
|-----------|-----|
| Single word input | `std::cin >> var` |
| Full line with spaces | `std::getline(std::cin, var)` |
| Full line after `>>` input | `std::getline(std::cin >> std::ws, var)` |

> Full coverage: [[Chapter 1 — C++ Basics]] → I/O / [[Chapter 5 — Constants and Strings]] → std::string
