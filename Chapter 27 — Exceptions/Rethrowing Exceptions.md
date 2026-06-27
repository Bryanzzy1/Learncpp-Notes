---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - concept
  - syntax
  - best-practice
aliases:
  - rethrow
  - rethrowing
  - throw; keyword
  - rethrow exception
up: "[[Chapter 27 — Exceptions]]"
related:
  - "[[Basic Exception Handling]]"
  - "[[Stack Unwinding]]"
  - "[[Exception Classes]]"
  - "[[Function Try Blocks]]"
  - "[[Object Slicing]]"
  - "[[Chapter 27 — Exceptions]]"
---

# Rethrowing Exceptions

**Rethrowing** allows a catch block to handle part of an exception (e.g. log it) and then pass it further up the call stack for another handler to deal with.

## Syntax

Use `throw;` with no argument inside a catch block to rethrow the currently-active exception:

```cpp
catch (Base& b)
{
    std::cout << "Caught Base b, which is actually a ";
    b.print();    // log some info
    std::cout << '\n';
    throw;        // rethrow the original exception unchanged
}
```

## Why `throw;` and not `throw b;`

`throw b;` would throw a **new copy** of `b` — sliced to the `Base` type (see [[Object Slicing]]). If the original exception was a `Derived` object caught by `Base&`, rethrowing it by name would lose the derived portion.

`throw;` rethrows the **exact original exception object** (the one stored in exception storage), preserving its full dynamic type. This is nearly always what you want.

## Common use cases

| Scenario | Pattern |
|---|---|
| Log and pass on | Log in the catch, then `throw;` |
| Translate and rethrow | Catch one type, throw a different (more appropriate) exception type |
| Constructor function try block | Must `throw;` or throw a new exception — cannot suppress (see [[Function Try Blocks]]) |

## Catch-then-rethrow in layers

Rethrowing is idiomatic when a function knows enough to enrich the error message or log context, but not enough to actually resolve the error:

```cpp
void processFile(const std::string& name)
{
    try { /* ... */ }
    catch (const std::exception& e)
    {
        std::cerr << "Error processing " << name << ": " << e.what() << '\n';
        throw; // let the caller decide how to recover
    }
}
```

> Full coverage: [[Chapter 27 — Exceptions]] → Rethrowing Exceptions
