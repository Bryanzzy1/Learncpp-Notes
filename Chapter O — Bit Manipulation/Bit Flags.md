---
tags:
  - cpp/bits
  - cpp/bitset
  - concept
  - syntax
aliases:
  - bit flag
  - flag
  - std::bitset
  - bitset
up: "[[Chapter O — Bit Manipulation]]"
related:
  - "[[Bitwise Operators]]"
  - "[[Bit Masks]]"
  - "[[Literals]]"
---

# Bit Flags

A **bit flag** is an individual bit of an object used as a Boolean value. A **flag** signals when some condition exists in a program.

## std::bitset

`std::bitset<N>` stores N bits and provides named operations for manipulating them.

```cpp
#include <bitset>

std::bitset<8> bits{ 0b0000'0101 }; // start with bit pattern 0000 0101
bits.set(3);   // turn bit 3 on  → 0000 1101
bits.flip(4);  // flip bit 4     → 0001 1101
bits.reset(4); // turn bit 4 off → 0000 1101
```

### Modification

| Method | Effect |
|--------|--------|
| `set(pos)` | Turn bit on (no-op if already on) |
| `reset(pos)` | Turn bit off (no-op if already off) |
| `flip(pos)` | Toggle bit |

### Querying

| Method | Returns |
|--------|---------|
| `test(pos)` | `true` if bit is 1 |
| `size()` | Total number of bits |
| `count()` | Number of bits set to `true` |
| `all()` | `true` if all bits are set |
| `any()` | `true` if any bit is set |
| `none()` | `true` if no bits are set |

> Full coverage: [[Chapter O — Bit Manipulation]] → Bit Flags
