---
tags:
  - cpp/types
  - cpp/char
  - concept
  - syntax
aliases:
  - char
  - character
  - escape sequence
  - ASCII
up: "[[Chapter 4 — Fundamental Data Types]]"
related:
  - "[[Strings]]"
  - "[[IO]]"
  - "[[Integers]]"
  - "[[Fundamental Data Types]]"
---

# Char

`char` is always exactly 1 byte. It stores an integer that maps to an ASCII character code.

```cpp
char c1 { 'a' };    // stores 97 (ASCII for 'a')
char c2 { 97  };    // same value — std::cout prints 'a'
```

- `signed char`: range −128 to 127
- `unsigned char`: range 0 to 255
- `std::cout` outputs the character symbol, not the integer value

**Escape sequences:**

| Sequence | Meaning |
|----------|---------|
| `\n` | newline |
| `\t` | horizontal tab |
| `\\` | backslash |
| `\'` | single quote |
| `\"` | double quote |
| `\a` | alert (beep) |
| `\r` | carriage return |

**String literals:**
- Text in double quotes (`"hello"`) is a **C-style string literal** — a null-terminated sequence of `char`
- Avoid multicharacter literals (`'56'`) — behavior is implementation-defined

> Full coverage: [[Chapter 4 — Fundamental Data Types]] → Char
