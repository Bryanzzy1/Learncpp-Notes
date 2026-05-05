---
tags:
  - cpp/io
  - concept
  - syntax
aliases:
  - std::cout
  - std::cin
  - std::cerr
  - buffered output
  - input output
  - iostream
up: "[[Chapter 1 — C++ Basics]]"
related:
  - "[[Debugging]]"
  - "[[Initialization]]"
  - "[[getline]]"
  - "[[Side Effects]]"
  - "[[Output Manipulators]]"
---

# I/O — std::cout, std::cin, std::cerr

## Output: std::cout

- **Buffered** — output collects in a buffer before being sent to the console
- The buffer is flushed periodically, or when forced with `std::endl`
- Use `\n` instead of `std::endl` to avoid an unnecessary flush on every line
- If the program crashes before the buffer flushes, buffered output is lost
- `operator<<` returns its left operand, enabling chaining — see [[Side Effects]]

## Input: std::cin

- **Buffered** — input is also buffered
- Leading whitespace is discarded before extraction with `>>`
- `operator>>` stops at the first whitespace — use [[getline|`std::getline()`]] for full-line input
- On extraction failure, the variable is set to `0`; subsequent extractions fail until `std::cin.clear()` is called

```cpp
std::string name;
std::getline(std::cin >> std::ws, name);  // std::ws discards leading whitespace
```

## Debug Output: std::cerr

- **Unbuffered** — always prints immediately, even if the program crashes mid-execution
- Preferred over `std::cout` for debug messages — see [[Debugging]]

> Full coverage: [[Chapter 1 — C++ Basics]] → I/O
