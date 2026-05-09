---
tags:
  - cpp/testing
  - concept
  - subnode
aliases:
  - statement coverage
up: "[[Code Coverage]]"
related:
  - "[[Code Coverage]]"
  - "[[Branch Coverage]]"
  - "[[Loop Coverage]]"
---

# Statement Coverage

**Statement coverage** is the percentage of statements in your code that have been executed by the test suite.

A statement is covered if it runs at least once during testing. Statement coverage is the most basic [[Code Coverage]] metric — a test suite can achieve 100% statement coverage while still missing entire branches (e.g., only taking the `true` path of an `if`).

For stronger guarantees, measure [[Branch Coverage]] in addition to statement coverage.

> Full coverage: [[Chapter 9 — Error handling]] → Statement Coverage
