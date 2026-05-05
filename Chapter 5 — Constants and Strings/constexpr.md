---
tags:
  - cpp/constexpr
  - cpp/constants
  - cpp/optimization
  - concept
  - syntax
aliases:
  - compile-time constant
  - constant expression
  - constexpr variable
  - constexpr function
  - compile-time evaluation
up: "[[Chapter 5 — Constants and Strings|Chapter 5 — Constants and Strings]]"
related:
  - "[[Initialization]]"
  - "[[Pass by Value]]"
  - "[[Preprocessing]]"
  - "[[constexpr Functions]]"
  - "[[Compile-time Evaluation]]"
---

# constexpr

A `constexpr` variable is always a **compile-time constant** — its value is fully determined during compilation, not at runtime.

```cpp
constexpr int x { 5 };         // OK: literal is a compile-time constant
constexpr int y { x + 1 };     // OK: x is constexpr
const int z { getUserInput() }; // runtime constant only — NOT constexpr
```

**Rules:**
- Must be initialized with an expression that is itself evaluatable at compile time
- [[Pass by Value|Function parameters]] cannot be `constexpr` — their value is determined at the call site
- [[constexpr Functions|`constexpr` functions]] can participate in constant expressions when all arguments are compile-time constants

**Compile-time optimizations enabled by constexpr:**
- **Constant folding** — compiler replaces `2 + 3` with `5`
- **Constant propagation** — compiler substitutes known constant values
- **Dead code elimination** — unreachable branches are removed

**`const` vs `constexpr`:**
| | `const` | `constexpr` |
|--|---------|-------------|
| Must be compile-time? | No | **Yes** |
| Can be runtime value? | Yes | No |
| Optimizer hint? | Yes | Stronger |

> Full coverage: [[Chapter 5 — Constants and Strings|Chapter 5 — Constants and Strings]] → Constant Expressions
