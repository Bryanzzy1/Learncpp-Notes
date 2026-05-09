---
tags:
  - cpp/testing
  - cpp/debugging
  - concept
  - best-practice
aliases:
  - informal testing
  - unit testing
  - test preservation
up: "[[Chapter 9 — Error handling]]"
related:
  - "[[Assertions]]"
  - "[[Code Coverage]]"
  - "[[Defensive Programming]]"
  - "[[Functions]]"
  - "[[Debugging]]"
---

# Informal Testing

**Informal testing** is the practice of writing ad-hoc code to verify a unit (function or class) immediately after implementing it.

## Throw-away vs. preserved tests

The simplest approach is to write a quick test, confirm it passes, then delete it. This is fast but fragile — the test is gone once it passes, and a future change can silently reintroduce the bug.

A better approach is to **preserve tests** so they can be re-run whenever the code changes. Preserved tests serve as a regression safety net.

## General practice

- Write programs in small, well-defined units ([[Functions|functions]] or classes).
- Compile often and test each unit as you go.
- Test different **categories** of input values (not just the happy path) to ensure the unit handles all cases correctly.

For more formal assertion-based testing, see [[Assertions]]. For measuring how thoroughly tests exercise the code, see [[Code Coverage]].

> Full coverage: [[Chapter 9 — Error handling]] → Informal Testing
