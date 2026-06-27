---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - concept
  - best-practice
aliases:
  - exception performance
  - exception downsides
  - when to use exceptions
  - exception cost
up: "[[Chapter 27 — Exceptions]]"
related:
  - "[[Basic Exception Handling]]"
  - "[[noexcept]]"
  - "[[Handling Errors in Functions]]"
  - "[[Assertions]]"
  - "[[Stack Unwinding]]"
  - "[[Chapter 27 — Exceptions]]"
---

# Exception Dangers

Exceptions are a powerful mechanism, but they come with trade-offs. Used incorrectly, they can make code harder to reason about, slower, and less safe.

## When exceptions are appropriate

Use exception handling when **all** of the following hold:

1. The error occurs **infrequently** (not the common path).
2. The error is **serious** — execution cannot meaningfully continue without handling it.
3. The error **cannot be handled** at the point where it occurs.
4. There is **no good alternative** (no practical sentinel value, no out-parameter).

For programmer errors and violated invariants that should never occur in correct code, use [[Assertions|assert()]] instead — exceptions are for runtime conditions the programmer cannot prevent.

## Performance costs

Exceptions have two distinct performance impacts:

| Phase | Cost |
|---|---|
| Normal execution (no throw) | Small overhead — the compiler must generate tables and slightly different code to support unwinding. Usually negligible. |
| When an exception is thrown | Significant — the runtime must unwind the stack (see [[Stack Unwinding]]), search for a matching handler, and copy the exception object. This is a relatively expensive operation. |

Exceptions are optimized for the **non-throwing path**. If exceptions are thrown frequently (e.g. as a normal control flow mechanism), performance suffers. This is why exceptions should represent exceptional — not routine — conditions.

## Safety concerns

- A thrown exception that escapes a **destructor** during stack unwinding causes `std::terminate()` — the program is killed immediately. Destructors must not throw. See `noexcept` in [[noexcept]].
- Uncaught exceptions also call `std::terminate()` without guaranteed stack unwinding, potentially leaking resources.
- Exceptions interact with [[Stack Unwinding]] and RAII: raw owning pointers will leak if an exception fires before `delete`. Prefer smart pointers or RAII wrappers.

## Exceptions vs error codes

| | Exceptions | Error codes / sentinels |
|---|---|---|
| Propagation | Automatic (stack unwinding) | Manual (every call site must check) |
| Overhead on success path | Minor | None |
| Overhead on error path | Higher | Minimal |
| Can be ignored by caller? | No (unhandled → terminate) | Yes (silent bugs) |
| Appropriate for deep call stacks | Yes | Awkward |

See [[Handling Errors in Functions]] for a full comparison of strategies.

> Full coverage: [[Chapter 27 — Exceptions]] → Exception Dangers
