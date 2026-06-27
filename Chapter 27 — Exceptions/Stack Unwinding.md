---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - concept
aliases:
  - stack unwinding
  - exception propagation
  - unwind the stack
  - call stack propagation
up: "[[Chapter 27 — Exceptions]]"
related:
  - "[[Basic Exception Handling]]"
  - "[[Uncaught Exceptions]]"
  - "[[Function Try Blocks]]"
  - "[[Destructors]]"
  - "[[The Stack and the Heap]]"
  - "[[Chapter 27 — Exceptions]]"
---

# Stack Unwinding

**Stack unwinding** is the process by which C++ propagates an exception up the call stack until a matching `catch` handler is found.

## How it works

When a `throw` executes and the current function has no matching `catch`:

1. The current function's local variables are destroyed (their [[Destructors|destructors]] execute).
2. Control returns to the calling function.
3. If that function has a matching `catch`, it handles the exception.
4. If not, that function's locals are also destroyed, and the process repeats up the call stack.

This continues until either a handler is found or the top of the stack is reached (see [[Uncaught Exceptions]]).

## Exceptions cross function boundaries

A `try` block catches exceptions not only from code directly inside it, but from any function called within it — no matter how deeply nested:

```cpp
void inner()
{
    throw std::runtime_error("failure"); // thrown here
}

void middle()
{
    inner(); // no try here — unwinding passes through
}

int main()
{
    try
    {
        middle(); // exception propagates out of middle(), caught here
    }
    catch (const std::runtime_error& e) { }
}
```

## Destructors run during unwinding

As each stack frame is destroyed, local objects have their destructors called in reverse construction order. This is the RAII guarantee: resources (files, locks, memory) held by local objects are released even when an exception is thrown.

Objects managed by [[Destructors|smart-pointer wrappers]] or RAII guards are therefore safe during unwinding. Raw owning pointers are not — they will leak if an exception fires before `delete` is reached.

## What is **not** re-executed

Unwinding does **not** return control to the `throw` site. Execution jumps directly to the matching `catch` block; code between the `throw` and the `catch` in the unwound frames is skipped (except for destructor calls).

> Full coverage: [[Chapter 27 — Exceptions]] → Stack Unwinding
