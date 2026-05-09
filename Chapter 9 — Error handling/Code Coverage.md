---
tags:
  - cpp/testing
  - cpp/debugging
  - concept
  - best-practice
aliases:
  - code coverage
  - test coverage
up: "[[Chapter 9 — Error handling]]"
related:
  - "[[Statement Coverage]]"
  - "[[Branch Coverage]]"
  - "[[Loop Coverage]]"
  - "[[Informal Testing]]"
  - "[[Control Flow]]"
  - "[[Loops]]"
---

# Code Coverage

**Code coverage** describes how much of a program's source code is actually executed by the test suite. Higher coverage means more paths through the code have been exercised, reducing the chance of untested bugs.

## Types of coverage

| Metric | What it measures |
|--------|-----------------|
| [[Statement Coverage]] | Percentage of individual statements executed |
| [[Branch Coverage]] | Percentage of conditional branches taken (each branch counted separately) |
| [[Loop Coverage]] | Whether loops execute for 0, 1, and 2+ iterations |

## Goal

Aim for **100% [[Branch Coverage]]** — every possible branch in your [[Control Flow]] should be taken by at least one test. Statement coverage alone is insufficient because it does not distinguish between the true and false arms of a conditional.

[[Loop Coverage]] (the 0, 1, 2 test) ensures that loop edge cases are covered. Testing different **categories** of input further strengthens coverage beyond what any single metric captures.

> Full coverage: [[Chapter 9 — Error handling]] → Code Coverage
