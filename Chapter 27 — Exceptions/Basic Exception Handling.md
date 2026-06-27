---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - concept
  - syntax
aliases:
  - exception handling
  - try block
  - catch block
  - throw statement
  - raise exception
  - try-catch
up: "[[Chapter 27 — Exceptions]]"
related:
  - "[[Stack Unwinding]]"
  - "[[Uncaught Exceptions]]"
  - "[[Exception Classes]]"
  - "[[Rethrowing Exceptions]]"
  - "[[Handling Errors in Functions]]"
  - "[[Assertions]]"
  - "[[Chapter 27 — Exceptions]]"
---

# Basic Exception Handling

C++ exceptions are a structured mechanism for signaling and handling error conditions that can't easily be passed back through a return value.

## The three keywords

| Keyword | Role |
|---|---|
| `throw` | Signals that an exception has occurred ("raises" it) |
| `try` | Marks a block to observe for thrown exceptions |
| `catch` | Defines a handler for a specific exception type |

## Syntax

```cpp
try
{
    // code that may fail
    throw -1;                          // throw an int
    throw "negative square root";      // throw a const char*
    throw MyException("Fatal Error");  // throw a class object
}
catch (int x)
{
    std::cerr << "Caught int: " << x << '\n';
}
catch (const MyException& e)
{
    std::cerr << "Caught: " << e.what() << '\n';
}
```

You can throw a value of almost any type: integers, enums, C-style strings, or class objects. Class objects (see [[Exception Classes]]) are the most idiomatic choice in modern C++.

## How exception handling works

1. A `throw` statement executes inside or beneath a `try` block.
2. The runtime searches backward up the call stack for the nearest enclosing `try` block with a matching `catch` handler.
3. Once a handler is found, execution jumps to the top of that `catch` block. The exception is considered **handled**.
4. Exceptions are handled immediately — control does not return to the throw site.

If no handler is found anywhere on the stack, `std::terminate()` is called (see [[Uncaught Exceptions]]).

## Multiple catch blocks

A single `try` can have multiple `catch` blocks, each for a different type. The first matching handler wins:

```cpp
catch (int x) { /* handles int */ }
catch (double d) { /* handles double */ }
catch (...) { /* catch-all — see Catch-All Handler */ }
```

## Relationship to other error strategies

Exceptions are one of several error-handling strategies (see [[Handling Errors in Functions]]). Prefer exceptions when:
- The error needs to propagate many levels up the call stack.
- A return-value sentinel is impractical or would pollute the API.

Prefer [[Assertions]] for programmer errors (precondition violations that should never occur in correct code).

> Full coverage: [[Chapter 27 — Exceptions]] → Basic Exception Handling
