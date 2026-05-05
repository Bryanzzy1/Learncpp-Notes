---
tags:
  - cpp/memory
  - concept
aliases:
  - memory addresses
  - memory allocation
  - memory deallocation
  - object lifetime
up: "[[Chapter 4 — Fundamental Data Types|Chapter 4 — Fundamental Data Types]]"
related:
  - "[[Initialization]]"
  - "[[Functions]]"
  - "[[size_t]]"
---

# Memory Model

Memory is organized into sequential units called **memory addresses**, each holding 1 byte.

- Every object occupies a contiguous block of memory addresses for its entire lifetime
- The type determines the byte count of that block — inspected with `sizeof`, which returns [[size_t]]
- When an object is [[Initialization|instantiated]], a portion of free memory is allocated to it
- When a local variable is destroyed on [[Functions|function]] exit, its memory is **deallocated** — freed for reuse by other objects

```
Memory (conceptual):
Address  Value
[0x01]   0b0000'0101   ← one byte of a variable
[0x02]   0b0000'0000
...
```

> Full coverage: [[Chapter 4 — Fundamental Data Types|Chapter 4 — Fundamental Data Types]] → Memory
