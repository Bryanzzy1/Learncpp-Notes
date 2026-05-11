---
tags:
  - cpp/types
  - cpp/casting
  - concept
aliases:
  - numeric promotion
  - integral promotion
  - floating point promotion
  - value-preserving conversion
  - safe conversion
up: "[[Chapter 10 — Type conversion]]"
related:
  - "[[Implicit Type Conversion]]"
  - "[[Standard Conversions]]"
  - "[[Numeric Conversions]]"
  - "[[Integers]]"
  - "[[Floating Point]]"
---

# Numeric Promotion

**Numeric promotion** is the conversion of a narrower numeric type to a wider type that the CPU can process efficiently. It is always **value-preserving** — every possible source value maps to an equal value in the destination type.

## Integral promotions

| Source type | Promoted to |
| --- | --- |
| `signed char`, `signed short` | `int` |
| `unsigned char`, `char8_t`, `unsigned short` | `int` (if `int` holds the full range), else `unsigned int` |
| `char` | Follows signed or unsigned rules depending on implementation |
| `bool` | `int` (`false` → `0`, `true` → `1`) |

## Floating-point promotions

`float` → `double`.

## Not all widening conversions are promotions

Only the specific conversions above qualify as numeric promotions. Other widening conversions (e.g. `int` → `long`) are [[Numeric Conversions]], not promotions — even though they may be value-preserving.

## Value-preserving vs. unsafe

A **value-preserving conversion** (also called a **safe conversion**) is one where every possible source value can be converted to an equal value in the destination type. Numeric promotions are always value-preserving. Numeric conversions may not be — see [[Narrowing Conversions]].

> Full coverage: [[Chapter 10 — Type conversion]] → Numeric Promotion
