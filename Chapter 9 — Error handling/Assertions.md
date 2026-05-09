---
tags:
  - cpp/testing
  - cpp/error-handling
  - cpp/debugging
  - concept
  - syntax
  - best-practice
aliases:
  - assert
  - assert()
  - NDEBUG
  - cassert
up: "[[Chapter 9 — Error handling]]"
related:
  - "[[static_assert]]"
  - "[[Informal Testing]]"
  - "[[Defensive Programming]]"
  - "[[Handling Errors in Functions]]"
  - "[[Halts]]"
  - "[[Undefined Behavior]]"
  - "[[Debugging]]"
---

# Assertions

An **assertion** (`assert()`) checks that a condition holds at runtime. If the condition is false, the program aborts with an error message pointing to the failed assertion — making bugs far easier to locate than silent [[Undefined Behavior]].

```cpp
#include <cassert> // for assert()

int divide(int a, int b)
{
    assert(b != 0); // abort if b is zero
    return a / b;
}
```

## Disabling assertions

Assertions are disabled by defining `NDEBUG` before including `<cassert>`:

```cpp
#define NDEBUG
#include <cassert>
```

To re-enable them, use `#undef NDEBUG`. In practice, build systems define `NDEBUG` automatically for release builds, so asserts have zero cost in production.

## When to use

Use `assert()` to document and enforce preconditions that should never be violated in correct code. Assertions catch **programmer errors** (logic bugs), not user input errors — for the latter, use proper error handling (see [[Handling Errors in Functions]]).

Because assertions can abort the program (similar to [[Halts|std::abort()]]), they are a form of [[Defensive Programming]].

## assert vs. static_assert

Prefer [[static_assert]] over `assert()` whenever the condition can be evaluated at compile time. `assert()` is for runtime invariants; `static_assert` catches errors before the program even runs.

> Full coverage: [[Chapter 9 — Error handling]] → Assertions
