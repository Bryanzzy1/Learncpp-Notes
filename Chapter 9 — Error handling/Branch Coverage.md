---
tags:
  - cpp/testing
  - concept
  - best-practice
  - subnode
aliases:
  - branch coverage
up: "[[Code Coverage]]"
related:
  - "[[Code Coverage]]"
  - "[[Statement Coverage]]"
  - "[[Loop Coverage]]"
  - "[[Control Flow]]"
  - "[[Conditional Statements]]"
---

# Branch Coverage

**Branch coverage** is the percentage of branches in your code that have been executed by the test suite, with each possible branch counted separately.

For an `if`/`else`, there are two branches: the true path and the false path. 100% statement coverage does not imply 100% branch coverage — you could cover every statement while only ever taking the `true` branch.

## Goal

Aim for **100% branch coverage**. Every possible outcome of every [[Conditional Statements|conditional]] in your [[Control Flow]] should be exercised by at least one test.

Branch coverage subsumes [[Statement Coverage]] — if every branch is taken, every reachable statement is also executed.

> Full coverage: [[Chapter 9 — Error handling]] → Branch Coverage
