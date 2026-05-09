---
tags:
  - cpp/error-handling
  - cpp/debugging
  - concept
  - best-practice
aliases:
  - error handling
  - sentinel value
  - fatal error
  - non-recoverable error
  - error propagation
up: "[[Chapter 9 — Error handling]]"
related:
  - "[[Defensive Programming]]"
  - "[[Assertions]]"
  - "[[Functions]]"
  - "[[Return by Value]]"
  - "[[Halts]]"
  - "[[Undefined Behavior]]"
---

# Handling Errors in Functions

When a [[Functions|function]] detects an error condition, it has several strategies available, each with different trade-offs.

## Strategy 1: Handle the error within the function

The function resolves the problem internally — for example, using a default value, retrying, or logging — and returns normally. This is appropriate only when the error is truly local and callers do not need to know about it.

## Strategy 2: Pass the error back to the caller

The function signals failure to its caller, which then decides how to respond.

Common mechanisms:
- **Return value** — return a special value indicating failure (e.g., `-1`, `nullptr`, `false`).
- **Sentinel value** — a value with special meaning in the context of a function or algorithm (e.g., `std::string::npos`, `-1` for "not found"). The caller must check for it.
- **Output parameter** — write a success/error flag to a by-reference parameter alongside the actual result.

See [[Return by Value]] for the mechanics of returning values to callers.

## Strategy 3: Fatal errors (non-recoverable)

If the error is severe enough that the program cannot continue operating correctly, it is a **non-recoverable** (or **fatal**) error. In this case, the appropriate response is to terminate — see [[Halts]] for `std::exit()` and `std::abort()`.

Use [[Assertions|assert()]] for programmer errors (precondition violations), which are also effectively fatal in debug builds.

## Strategy 4: Exceptions

C++ exceptions (`try` / `throw` / `catch`) provide a structured mechanism for propagating errors up the call stack without explicitly threading error codes through every return value. Exceptions are the idiomatic choice for error handling in modern C++ when the error needs to travel far from its source.

## Choosing a strategy

| Situation | Preferred strategy |
|-----------|-------------------|
| Error is internal and insignificant | Handle within function |
| Caller needs to know, simple interface | Return sentinel / error code |
| Error may need to propagate many levels | Exceptions |
| Programmer bug / violated invariant | `assert()` / `static_assert` |
| Program cannot meaningfully continue | Fatal halt (`std::abort`) |

Unhandled or ignored errors can lead to [[Undefined Behavior]] or silent data corruption — always decide consciously which strategy applies.

> Full coverage: [[Chapter 9 — Error handling]] → Handling Errors in Functions
