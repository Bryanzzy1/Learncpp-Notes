---
tags:
  - cpp/io
  - concept
  - syntax
  - subnode
aliases:
  - output manipulator
  - std::setprecision
  - std::boolalpha
  - std::hex
  - std::oct
  - std::dec
  - std::endl
  - iomanip
up: "[[IO]]"
related:
  - "[[IO]]"
  - "[[Floating Point]]"
  - "[[Boolean]]"
  - "[[Literals]]"
---

# Output Manipulators

Output manipulators are functions passed to `operator<<` that change how subsequent output is formatted. Most are persistent — they stay active until changed.

## Floating-Point Precision

```cpp
#include <iomanip>
std::cout << std::setprecision(4) << 3.14159;  // prints 3.142
```

`std::setprecision(n)` sets the number of significant digits displayed. Requires `<iomanip>`.

## Boolean Display

```cpp
std::cout << std::boolalpha << true;   // prints "true" instead of "1"
std::cin  >> std::boolalpha >> flag;   // accepts "true"/"false" as input
```

## Integer Base

```cpp
std::cout << std::hex << 255;    // ff
std::cout << std::oct << 255;    // 377
std::cout << std::dec << 255;    // 255 (default)
```

## Binary Output

No built-in binary manipulator before C++20. Use `std::bitset`:

```cpp
#include <bitset>
std::cout << std::bitset<8>{ 0b1010'1100 };   // 10101100
```

C++20+: `std::format("{:b}", x)` or `std::format("{:#b}", x)` for `0b`-prefixed output.

## Flushing

```cpp
std::cout << std::endl;   // outputs '\n' AND flushes the buffer — slow
std::cout << '\n';        // outputs '\n' only — preferred
```

> Full coverage: [[Chapter 4 — Fundamental Data Types]] → Floats / Boolean | [[Chapter 5 — Constants and Strings]] → Literals
