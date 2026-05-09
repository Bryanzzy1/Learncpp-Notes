---
tags:
  - cpp/testing
  - cpp/loops
  - concept
  - best-practice
  - subnode
aliases:
  - loop coverage
  - 0 1 2 test
  - zero one two test
up: "[[Code Coverage]]"
related:
  - "[[Code Coverage]]"
  - "[[Statement Coverage]]"
  - "[[Branch Coverage]]"
  - "[[Loops]]"
  - "[[For Loop]]"
  - "[[Do-While Loop]]"
---

# Loop Coverage

**Loop coverage** (informally called **the 0, 1, 2 test**) ensures that a loop is tested for three iteration counts:

| Iterations | What it catches |
|-----------|----------------|
| **0** | Loop body never runs — initialization/post-loop logic is correct |
| **1** | Loop runs exactly once — single-pass behavior is correct |
| **2** | Loop runs multiple times — iteration logic (increment, condition) is correct |

If a loop works correctly for 2 iterations, it should work correctly for all counts greater than 2 — the inductive case is covered.

Loop coverage is a supplement to [[Branch Coverage]] because loops contain implicit branches (the continue/exit decision on each iteration). It applies to `for`, `while`, and [[Do-While Loop|do-while]] loops (see [[Loops]]).

> Full coverage: [[Chapter 9 — Error handling]] → Loop Coverage
