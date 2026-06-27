---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - cpp/classes
  - concept
  - syntax
  - best-practice
aliases:
  - function try block
  - function-level try
  - constructor try block
  - function level catch
up: "[[Chapter 27 — Exceptions]]"
related:
  - "[[Basic Exception Handling]]"
  - "[[Rethrowing Exceptions]]"
  - "[[Stack Unwinding]]"
  - "[[Constructors]]"
  - "[[Member Initializer List]]"
  - "[[Destructors]]"
  - "[[Undefined Behavior]]"
  - "[[Chapter 27 — Exceptions]]"
---

# Function Try Blocks

A **function try block** wraps the entire body of a function — and for constructors, the [[Member Initializer List|member initializer list]] — in a single try/catch, rather than wrapping a specific block of code inside the function.

## Syntax

```cpp
class B : public A
{
public:
    B(int x) try : A{x}   // "try" appears before the initializer list
    {
        // constructor body
    }
    catch (...)            // at the same indentation level as the function
    {
        // catches exceptions from both the initializer list and the constructor body
        std::cerr << "Exception caught\n";
        throw;             // must rethrow or throw a new exception
    }
};
```

## Primary use case: constructor initializer lists

The [[Member Initializer List|member initializer list]] runs before the constructor body, and a `try` block inside the constructor body cannot catch exceptions thrown there. A function try block is the only way to intercept them.

## Behavior per function type

| Function type | Can resolve the exception (via `return`)? | Behavior if catch block ends without throw |
|---|---|---|
| Constructor | No — must throw or rethrow | Implicit rethrow |
| Destructor | Yes | Implicit rethrow |
| Non-value-returning function (`void`) | Yes | Exception resolved |
| Value-returning function | Yes | [[Undefined Behavior]] |

**Constructors cannot suppress exceptions** — the object is not yet fully constructed, so allowing construction to "succeed" despite a failure would leave the program in an invalid state. Any constructor function try block must either `throw` a new exception or rethrow (`throw;`) the existing one.

## What function try blocks cannot do

- **Do not use them to clean up resources** — by the time the catch runs during [[Stack Unwinding|stack unwinding]], member objects that were successfully constructed will have already had their destructors called. Performing additional cleanup risks double-destruction.
- An exception thrown from a **destructor during stack unwinding** cannot be caught by a function try block on the same destructor — the program halts if a destructor throws during unwinding.

## Avoid falling off the end

Letting control reach the end of a function-level catch block without a `throw`, `return`, or explicit `std::terminate()` produces implementation-defined behavior (or [[Undefined Behavior]] for value-returning functions). Always be explicit.

> Full coverage: [[Chapter 27 — Exceptions]] → Function Try Blocks
