---
tags:
  - cpp/types
  - cpp/casting
  - concept
aliases:
  - numeric conversion
  - value-preserving conversion
  - reinterpretive conversion
  - lossy conversion
up: "[[Chapter 10 — Type conversion]]"
related:
  - "[[Implicit Type Conversion]]"
  - "[[Standard Conversions]]"
  - "[[Numeric Promotion]]"
  - "[[Narrowing Conversions]]"
  - "[[Integers]]"
  - "[[Floating Point]]"
---

# Numeric Conversions

**Numeric conversions** are [[Implicit Type Conversion|implicit]] conversions between integral and floating-point types that do not qualify as [[Numeric Promotion|numeric promotions]]. They are performed to represent a value in the target type, not for performance.

## The five basic types

1. Integral type → any other integral type (excluding promotions)
2. Floating-point type → any other floating-point type (excluding promotions)
3. Floating-point type → integral type
4. Integral type → floating-point type
5. Integral or floating-point type → `bool`

## Safety categories

| Category | Description | Example |
| --- | --- | --- |
| Value-preserving | Destination can represent all source values exactly | `int` → `long`, `short` → `double` |
| Reinterpretive | Converted value may differ, but no bits are lost | Signed ↔ unsigned conversions |
| Lossy | Data may be lost | `double` → `int` (truncation), `double` → `float` |

C++20 requires signed integers to use two's complement, making signed/unsigned reinterpretation well-defined.

A conversion that can lose data is called an **unsafe conversion**. The specific unsafe conversions that appear in brace initialization are called [[Narrowing Conversions]].

> Full coverage: [[Chapter 10 — Type conversion]] → Numeric Conversions
