---
tags:
  - cpp/control-flow
  - concept
aliases:
  - execution path
  - branching
  - control flow statement
  - flow control
up: "[[Chapter 8 — Control Flow]]"
related:
  - "[[Conditional Statements]]"
  - "[[Switch Statements]]"
  - "[[Goto Statements]]"
  - "[[Loops]]"
  - "[[Halts]]"
  - "[[Functions]]"
---

# Control Flow

When a program runs, the CPU begins execution at the top of `main()`. The specific sequence of statements the CPU executes is the program's **execution path**.

**Straight-line programs** take the same path every run. When a control flow statement causes execution to jump to a non-sequential statement, this is called **branching**.

## Categories of control flow

| **Category** | **Meaning** | **C++ keywords** |
| --- | --- | --- |
| Conditional statements | Execute code only if some condition is met | `if`, `else`, `switch` |
| Jumps | Execute statements at another location | `goto`, `break`, `continue` |
| Function calls | Jump to another location and back | function calls, `return` |
| Loops | Repeatedly execute code zero or more times | `while`, `do-while`, `for`, range-`for` |
| Halts | Terminate the program | `std::exit()`, `std::abort()` |
| Exceptions | Error-handling flow control | `try`, `throw`, `catch` |

See [[Conditional Statements]] and [[Switch Statements]] for branching, [[Loops]] for repetition, [[Goto Statements]] for unconditional jumps, [[Halts]] for program termination, and [[Functions]] for call/return mechanics.

> Full coverage: [[Chapter 8 — Control Flow]] → Control Flow
