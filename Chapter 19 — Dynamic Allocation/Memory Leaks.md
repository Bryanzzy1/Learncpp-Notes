---
tags:
  - cpp/memory
  - concept
  - best-practice
  - subnode
aliases:
  - memory leak
  - leaked memory
up: "[[new and delete]]"
related:
  - "[[new and delete]]"
  - "[[RAII]]"
  - "[[Undefined Behavior]]"
  - "[[Memory Model]]"
  - "[[std-vector|std::vector]]"
---

# Memory Leaks

A **memory leak** occurs when a program loses the address of dynamically allocated memory before calling `delete` on it. The heap block becomes permanently unreachable — neither the program nor the OS can reclaim it until the process exits.

## Common causes

1. **Overwriting the only pointer** to a heap object before deleting it:

```cpp
int* ptr{ new int{5} };
ptr = new int{10};  // original block is now leaked
```

2. **Early return or exception** before the `delete` call is reached.
3. **Missing `delete` in a loop** — leaks accumulate per iteration.

## Consequences

Repeated leaks drain available heap memory, eventually causing allocation failures. In long-running programs (servers, daemons) even small leaks become critical over time.

## Prevention

Prefer [[RAII]]-based containers ([[std-vector|std::vector]], smart pointers) that call `delete` in their destructors automatically. Raw `new`/`delete` pairs are error-prone and should be avoided in modern C++.

> Full coverage: [[Chapter 19 — Dynamic Allocation]] → Memory Leaks
