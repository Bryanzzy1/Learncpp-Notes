---
tags:
  - cpp/random
  - concept
  - syntax
  - subnode
aliases:
  - mt19937
  - mt19937_64
  - std::mt19937
  - uniform_int_distribution
  - std::uniform_int_distribution
  - std::random_device
up: "[[Pseudo-random Number Generators]]"
related:
  - "[[Pseudo-random Number Generators]]"
  - "[[Random Number Generation]]"
---

# Mersenne Twister

The **Mersenne Twister** is the standard C++ [[Pseudo-random Number Generators|PRNG]], provided in `<random>`. It is widely used because of its large state (19937 bits) and good statistical properties.

```cpp
#include <random>
```

| Type | Output |
|------|--------|
| `std::mt19937` | 32-bit unsigned integers |
| `std::mt19937_64` | 64-bit unsigned integers |

## Generating a value in a range

Use `std::uniform_int_distribution` to map the raw output to a desired range:

```cpp
std::mt19937 mt{ std::random_device{}() }; // seed with random_device
std::uniform_int_distribution<int> die6{ 1, 6 }; // [1, 6] inclusive

std::cout << die6(mt) << '\n';
```

## Seeding

Use `std::random_device{}()` to obtain a high-quality seed from the OS:

```cpp
std::mt19937 mt{ std::random_device{}() };
```

`std::random_device{}()` creates a value-initialized temporary `std::random_device` and immediately calls it to produce a non-deterministic seed value.

**Only seed a given PRNG once** — do not reseed it during the program. Reseeding can introduce correlation and reduce output quality.

> Full coverage: [[Chapter 8 — Control Flow]] → Mersenne Twister
