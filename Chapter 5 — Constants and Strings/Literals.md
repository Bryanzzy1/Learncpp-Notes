---
tags:
  - cpp/literals
  - cpp/types
  - concept
  - syntax
aliases:
  - literal suffix
  - binary literal
  - digit separator
  - std::bitset
  - hex literal
  - octal literal
up: "[[Chapter 5 — Constants and Strings]]"
related:
  - "[[Constants]]"
  - "[[Integers]]"
  - "[[Floating Point]]"
  - "[[IO]]"
---

# Literals

A **literal** is a fixed value written directly in source code. Every literal has a type, deduced from its form.

## Literal Suffixes

Override the default type of a literal:

| Suffix | Resulting type |
|--------|---------------|
| `u` / `U` | `unsigned int` |
| `l` / `L` | `long` |
| `ll` / `LL` | `long long` |
| `f` / `F` | `float` |
| `s` | `std::string` |
| `sv` | `std::string_view` |
| `uz` / `UZ` | `std::size_t` (C++23) |

- Prefer uppercase `L` over lowercase `l` — `l` is easily confused with `1`.

## Binary Literals (C++14)

Prefix `0b` for binary: `0b1010'1100`

## Digit Separators (C++14)

Use `'` between digits for readability: `1'000'000`, `0b1011'0010`

## Printing in Different Bases

```cpp
std::cout << std::hex << x;       // hexadecimal
std::cout << std::oct << x;       // octal
std::cout << std::bitset<8>{x};   // binary via std::bitset
```

For C++20/23, use `std::format("{:b}", x)` or `std::println("{:#b}", x)` for binary output.

> Full coverage: [[Chapter 5 — Constants and Strings]] → Literals
