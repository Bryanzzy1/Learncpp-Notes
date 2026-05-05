---
tags:
  - cpp/debugging
  - cpp/tools
  - best-practice
  - concept
aliases:
  - debugger
  - debug build
  - breakpoints
  - stepping
  - call stack
  - watch window
up: "[[Chapter 3 — Debugging C++ Programs|Chapter 3 — Debugging C++ Programs]]"
related:
  - "[[IO]]"
  - "[[Functions]]"
  - "[[Static Analysis]]"
---

# Debugging

## Debugger

### Stepping
- **Step into** — execute the next statement; enters called [[Functions|functions]]
- **Step over** — execute the next statement; runs an entire function without stepping inside
- **Step out** — finish the current function and return control to the caller

### Execution Control
- **Breakpoint** — pauses execution at a marked line when running in debug mode
- **Run to cursor** — runs until the selected line
- **Continue** — resumes normal execution until the next breakpoint or program end
- **Set next statement** — jumps execution to a different line; variables retain their current values

### Inspection
- **Watch window** — displays selected variables, updated at each step
- **Call stack** — lists every active function frame currently on the stack, with the next line to execute in each

## Debug Builds

Debug builds turn off compiler optimizations, so runtime behavior exactly matches what you wrote:
- Constant folding, constant propagation, and dead code elimination (see [[constexpr]]) are disabled
- Variable values are not optimized away and remain inspectable

## Print Debugging

Use `std::cerr` instead of `std::cout` — `cerr` is unbuffered and prints immediately even if the program crashes before the output buffer flushes. See [[IO]].

## [[Static Analysis|Static Analysis Tools]]
- [clang-tidy](https://clang.llvm.org/extra/clang-tidy/)
- [cppcheck](https://cppcheck.sourceforge.io/)
- [cpplint](https://github.com/cpplint/cpplint)

> Full coverage: [[Chapter 3 — Debugging C++ Programs|Chapter 3 — Debugging C++ Programs]]
