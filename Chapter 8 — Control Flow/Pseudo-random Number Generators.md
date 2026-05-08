---
tags:
  - cpp/random
  - concept
  - subnode
aliases:
  - PRNG
  - pseudo-random number generator
  - random seed
  - seeding
  - underseeded
up: "[[Random Number Generation]]"
related:
  - "[[Random Number Generation]]"
  - "[[Mersenne Twister]]"
---

# Pseudo-random Number Generators

A **pseudo-random number generator (PRNG)** is a stateful algorithm that produces a sequence of numbers whose statistical properties simulate true randomness. The output is entirely deterministic — the same seed always produces the same sequence.

## Seeding

The **seed** (or **random seed**) is the value (or set of values) used to set the PRNG's initial state:

- Once seeded, all subsequent values are determined by the seed — the PRNG is **seeded**.
- The same seed always produces the same output sequence.

### Underseeding

If a PRNG is not given enough quality bits as seed data, it is **underseeded**. Underseeding degrades the quality of the generated sequence. An ideal seed satisfies:

- At least as many bits as the PRNG's internal state.
- Each bit independently randomized.
- A good mix of 0s and 1s across all bits — no "stuck bits" (always 0 or always 1).
- Low correlation with previously generated seeds.

## Subnode

- [[Mersenne Twister]] — the standard C++ PRNG, provided via `<random>`

> Full coverage: [[Chapter 8 — Control Flow]] → Pseudo-random Number Generators
