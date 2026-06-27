---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - concept
  - best-practice
aliases:
  - uncaught exception
  - unhandled exception
  - std::terminate
  - terminate
up: "[[Chapter 27 — Exceptions]]"
related:
  - "[[Catch-All Handler]]"
  - "[[Basic Exception Handling]]"
  - "[[Stack Unwinding]]"
  - "[[Halts]]"
  - "[[Destructors]]"
  - "[[Chapter 27 — Exceptions]]"
---

# Uncaught Exceptions

If an exception propagates all the way up the call stack without finding a matching `catch` handler, the C++ runtime calls `std::terminate()`, which typically aborts the program.

## Stack-unwinding behavior with unhandled exceptions

The C++ standard does **not** guarantee that the stack is unwound before `std::terminate()` is called for an unhandled exception. In practice:
- Some implementations do unwind (running destructors).
- Others call `std::terminate()` immediately, leaving stack frames intact for debugger inspection.

Either way, local variables in unwound frames may not be properly cleaned up — making unhandled exceptions a potential resource leak even for RAII-managed objects. See [[Destructors]] and [[Stack Unwinding]].

## Preventing termination

Use a [[Catch-All Handler|catch-all handler]] (`catch(...)`) to intercept any exception type that slips through:

```cpp
int main()
{
    try
    {
        // application code
    }
    catch (...)  // catch-all
    {
        std::cerr << "Unhandled exception — aborting.\n";
        // optionally rethrow or call std::terminate() explicitly
    }
}
```

Wrapping `main()` with a catch-all prevents silent termination. See [[Catch-All Handler]] for details and the `NDEBUG` debugging pattern.

## Relationship to `std::terminate`

`std::terminate()` is also called in other situations:
- An exception is thrown from a `noexcept` function (see [[noexcept]]).
- An exception escapes a destructor during [[Stack Unwinding|stack unwinding]].

In all these cases the program is terminated without further exception handling.

## Subnode

- [[Catch-All Handler]] — `catch(...)` syntax, wrapping main, debug vs release patterns

> Full coverage: [[Chapter 27 — Exceptions]] → Uncaught Exceptions
