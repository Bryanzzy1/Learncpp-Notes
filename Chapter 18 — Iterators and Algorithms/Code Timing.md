---
tags:
  - cpp/debugging
  - cpp/best-practice
  - concept
  - best-practice
aliases:
  - timing code
  - performance measurement
  - benchmarking
  - release build
up: "[[Chapter 18 — Iterators and Algorithms]]"
related:
  - "[[Random Number Generation]]"
  - "[[Standard Library Algorithms]]"
---

# Code Timing

Measuring execution time accurately requires eliminating confounding variables. Misleading results are worse than no measurement.

## Prerequisites for valid measurements

Before timing:

| Factor | Correct state |
|---|---|
| Build configuration | **Release build**, not Debug — debug builds include extra checks and disable optimizations |
| System load | No CPU/memory/disk-intensive background tasks (games, antivirus, file indexing, OS updates) |
| Random number sequences | Be aware that different [[Random Number Generation|RNG]] seeds produce different workloads — fix the seed when comparing runs |
| User input | Do not include time spent waiting for user input |

## Gathering results

Collect **at least 3 measurements** and average them. Single-sample timing is unreliable due to OS scheduling jitter and CPU frequency variation.

> Full coverage: [[Chapter 18 — Iterators and Algorithms]] → Timing your code
