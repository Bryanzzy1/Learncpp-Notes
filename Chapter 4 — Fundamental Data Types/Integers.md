---
tags:
  - cpp/types
  - cpp/integers
  - concept
  - syntax
aliases:
  - signed integer
  - unsigned integer
  - integer overflow
  - fixed-width integer
  - std::int32_t
  - overflow
up: "[[Chapter 4 — Fundamental Data Types]]"
related:
  - "[[static_cast]]"
  - "[[size_t]]"
  - "[[Memory Model]]"
  - "[[Fundamental Data Types]]"
---

# Integers

C++ integer types represent whole numbers and are **signed by default**.

## Signed Integers

An n-bit signed integer holds values from −(2ⁿ⁻¹) to (2ⁿ⁻¹)−1.

| Type | Minimum size |
|------|-------------|
| `short` | 16 bits |
| `int` | 16 bits (typically 32) |
| `long` | 32 bits |
| `long long` | 64 bits |

## Unsigned Integers

No negative values. Out-of-range values **modulo-wrap**: divide by (max value + 1) and keep the remainder.

- Prefer **signed** over unsigned for quantities — mixing signed and unsigned integers causes subtle bugs and compiler warnings.

## Overflow

Signed overflow (result outside representable range) is **undefined behavior**.
Unsigned overflow is defined: it wraps modulo 2ⁿ.

## Fixed-Width Integers (`<cstdint>`)

Guarantee exact sizes across all platforms:

| Type | Size | Range |
|------|------|-------|
| `std::int8_t` | 1 byte signed | −128 to 127 |
| `std::int16_t` | 2 bytes signed | −32 768 to 32 767 |
| `std::int32_t` | 4 bytes signed | −2 147 483 648 to 2 147 483 647 |
| `std::int64_t` | 8 bytes signed | ≈ ±9.2 × 10¹⁸ |
| `std::uint8_t`–`std::uint64_t` | unsigned equivalents | 0 to 2ⁿ−1 |

- `std::int8_t` / `std::uint8_t` are often treated as `signed char` / `unsigned char` by compilers — avoid using them for numeric I/O.

## Fast and Least Types

- `std::int_fast32_t` — fastest signed integer ≥ 32 bits on the current platform
- `std::uint_least32_t` — smallest unsigned integer ≥ 32 bits

Avoid — their size varies by architecture, making code non-portable.

> Full coverage: [[Chapter 4 — Fundamental Data Types]] → Signed Ints / Unsigned Ints / Fixed-width integers
