---
tags:
  - cpp/random
  - concept
aliases:
  - random numbers
  - RNG
  - stateful algorithm
  - stateless algorithm
  - algorithm state
up: "[[Chapter 8 — Control Flow]]"
related:
  - "[[Pseudo-random Number Generators]]"
  - "[[Mersenne Twister]]"
  - "[[Control Flow]]"
  - "[[Static Local Variables]]"
---

# Random Number Generation

## Algorithms and state

An **algorithm** is a finite sequence of instructions that produces a useful result. Algorithms can be classified by whether they retain information between calls:

- A **stateful** algorithm retains some information across calls; its output depends on prior calls.
- A **stateless** algorithm needs all the information provided on each call; it produces the same output given the same input.

The term **state** refers to the current values held in the stateful variables retained across calls — similar to how [[Static Local Variables]] persist between function invocations.

## Why true randomness is hard

Computers are deterministic: given the same inputs, they produce the same outputs. Truly random numbers require a hardware source of entropy (thermal noise, radioactive decay, etc.). In most programs, **pseudo-random numbers** — numbers that merely appear random — are sufficient.

## Subnode

- [[Pseudo-random Number Generators]] — deterministic algorithms that simulate randomness via seeding

> Full coverage: [[Chapter 8 — Control Flow]] → Random Number Generation
