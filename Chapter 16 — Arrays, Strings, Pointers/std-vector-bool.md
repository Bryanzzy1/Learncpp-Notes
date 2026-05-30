---
tags:
  - cpp/vectors
  - cpp/containers
  - best-practice
  - subnode
aliases:
  - std::vector<bool>
  - vector bool
  - vector of bool
up: "[[std-vector]]"
related:
  - "[[std-vector]]"
  - "[[constexpr]]"
  - "[[Strings]]"
---

# std::vector\<bool\>

`std::vector<bool>` is a **space-optimized specialization** of `std::vector` that packs boolean values one bit per element instead of one byte. This specialization breaks the usual `std::vector` contract in subtle ways:

- `operator[]` and iterators return a **proxy object**, not a `bool&` reference.
- The proxy object does not behave like a normal reference in all contexts (e.g., taking its address does not yield a `bool*`).
- Code that works correctly with `std::vector<int>` may silently misbehave with `std::vector<bool>`.

## Preferred alternatives

| Use case | Alternative |
|----------|-------------|
| Compile-time fixed-size bitset | `constexpr std::bitset<N>` (see [[constexpr]]) |
| Runtime boolean sequence | `std::vector<char>` or `std::vector<uint8_t>` |
| Dynamic bitset | Third-party library (e.g., Boost.DynamicBitset) |

Avoid `std::vector<bool>` in new code. Its performance benefit is rarely worth the interface inconsistency.

> Full coverage: [[Chapter 16 — Arrays, Strings, Pointers]] → std::vector\<bool\>
