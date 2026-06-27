---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - concept
  - syntax
  - subnode
aliases:
  - catch-all
  - "catch(...)"
  - ellipsis catch
  - catch all handler
up: "[[Uncaught Exceptions]]"
related:
  - "[[Uncaught Exceptions]]"
  - "[[Basic Exception Handling]]"
  - "[[Conditional Compilation]]"
  - "[[Chapter 27 — Exceptions]]"
---

# Catch-All Handler

A **catch-all handler** uses the ellipsis (`...`) as its parameter to match any exception type, regardless of what was thrown:

```cpp
catch (...) // catches everything
{
    std::cerr << "We caught an exception of an undetermined type\n";
}
```

It must be placed **last** in a sequence of `catch` blocks, since the first matching handler wins and a catch-all would swallow all subsequent types.

## Wrapping main()

A common pattern is to wrap the entire body of `main()` in a `try` block with a catch-all, preventing unhandled exceptions from reaching `std::terminate()`:

```cpp
int main()
{
    try
    {
        // all application logic
    }
    catch (const std::exception& e)
    {
        std::cerr << "Exception: " << e.what() << '\n';
    }
    catch (...)
    {
        std::cerr << "Unknown exception — aborting.\n";
    }
}
```

## Debug vs release pattern

Using [[Conditional Compilation|conditional compilation]] with `NDEBUG`, you can compile in a catch-all for release builds (for graceful termination) while leaving exceptions unhandled in debug builds (for debugger stack inspection):

```cpp
#ifndef NDEBUG
    catch (...)
    {
        std::cerr << "Abnormal termination\n";
    }
#else
    catch (DummyException) { } // syntactic placeholder — never matched
#endif
```

## Limitations

The catch-all gives you no information about what was caught. If you need the exception's type or message, prefer catching `const std::exception&` first (see [[std::exception]]) and fall back to `catch(...)` only for non-standard throws.

> Full coverage: [[Chapter 27 — Exceptions]] → Uncaught Exceptions → Catch-All Handler
