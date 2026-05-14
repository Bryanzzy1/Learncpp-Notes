---
tags:
  - cpp/constexpr
  - cpp/functions
  - concept
  - best-practice
aliases:
  - pure function
  - side-effect-free function
up: "[[Chapter F — Constexpr functions]]"
related:
  - "[[Constexpr Function Rules]]"
  - "[[Consteval]]"
  - "[[constexpr]]"
  - "[[Functions]]"
  - "[[Global Variables]]"
  - "[[Static Local Variables]]"
---

# Pure Functions

A **pure function** satisfies two criteria:

1. **Deterministic** — always returns the same result given the same arguments
2. **No side effects** — does not modify static local variables, global variables, or perform I/O

```cpp
// Pure: same input always yields same output, no side effects
constexpr int square(int x) { return x * x; }

// Not pure: reads from a file (side effect / non-deterministic)
int readValue() { return std::cin.get(); }

// Not pure: modifies a static local (side effect)
int counter() { static int n { 0 }; return ++n; }
```

## Why it matters

Pure functions are **safe to evaluate at compile time** because they have no observable side effects that could change program state. This is why:

- Pure functions should generally be made [[Constexpr Function Rules|`constexpr`]]
- [[Consteval|`consteval`]] functions are always pure by necessity

Compilers can freely **cache**, **reorder**, or **eliminate** calls to pure functions under the as-if rule (see [[Compile-time Evaluation]]).

A function that modifies [[Global Variables|global variables]] or [[Static Local Variables|static local variables]], reads input, or writes output is **not** pure and cannot be `constexpr`.

> Full coverage: [[Chapter F — Constexpr functions]] → Pure Functions
