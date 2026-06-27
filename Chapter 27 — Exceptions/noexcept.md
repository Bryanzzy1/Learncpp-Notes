---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - concept
  - syntax
  - best-practice
aliases:
  - noexcept specifier
  - noexcept operator
  - exception safety
  - exception safety guarantee
  - non-throwing function
  - potentially throwing function
  - no-throw guarantee
  - strong guarantee
  - basic guarantee
  - no-fail guarantee
up: "[[Chapter 27 — Exceptions]]"
related:
  - "[[std-move-if-noexcept|std::move_if_noexcept]]"
  - "[[Move Constructor and Assignment]]"
  - "[[Destructors]]"
  - "[[Exception Dangers]]"
  - "[[Stack Unwinding]]"
  - "[[Chapter 27 — Exceptions]]"
---

# noexcept

`noexcept` is C++11's mechanism for declaring that a function will not throw exceptions visible to its caller. It serves as both a **specifier** (on the function declaration) and an **operator** (for querying at compile time).

## noexcept specifier

```cpp
void doSomething() noexcept;           // non-throwing
void doSomething() noexcept(true);     // equivalent

void doSomethingElse() noexcept(false); // potentially throwing (default for most functions)
```

If a `noexcept` function throws anyway, the runtime calls `std::terminate()` — there is no unwinding.

### Conditional noexcept (for templates)

The boolean form is useful in templates to propagate the noexcept-ness of an inner operation:

```cpp
template <typename T>
void swap(T& a, T& b) noexcept(noexcept(T(std::move(a))));
```

## Default noexcept classification

| Function type | Default |
|---|---|
| Destructors | Implicitly `noexcept` |
| Default / copy / move constructors (implicitly declared or `= default`) | `noexcept` if all members are |
| Copy / move assignment (implicitly declared or `= default`) | `noexcept` if all members are |
| Comparison operators as of C++20 (defaulted) | `noexcept` |
| Regular functions | Potentially throwing |
| User-defined constructors and operators | Potentially throwing |

## noexcept operator

The `noexcept(expr)` **operator** (without a function declaration) evaluates to `true` or `false` at compile time based on whether `expr` is considered non-throwing:

```cpp
bool b = noexcept(std::string{});   // true if string's default constructor is noexcept
```

Used inside `noexcept(noexcept(...))` patterns to conditionally declare a function noexcept.

## Exception safety guarantees

Code is said to satisfy one of four **exception safety guarantees**:

| Level | Guarantee |
|---|---|
| **No guarantee** | If an exception is thrown, anything can happen — the object may be left in an unusable state |
| **Basic guarantee** | No memory leaks; the object is still usable, but may be in a modified (indeterminate) state |
| **Strong guarantee** | No memory leaks; if an exception is thrown, the program state is unchanged — the operation either fully succeeds or has no side effects |
| **No-throw / No-fail** | The function always succeeds (no-fail) or never exposes exceptions to the caller (no-throw). `noexcept` maps to this level. |

## Best practices

- **Always mark move constructors, move assignment, and swap `noexcept`** — this allows the standard library (e.g. `std::vector::resize`) to choose the faster move path instead of copying for exception safety.
- Mark copy constructors and copy assignment `noexcept` when you can.
- Use `noexcept` on any function you are confident will never throw, to document the no-throw guarantee and enable optimizations.

## Legacy: dynamic exception specifications

Before C++17, the `throw(TypeList)` syntax documented which exceptions a function could throw. This was deprecated in C++11 and removed in C++17. Modern code uses `noexcept` instead.

## Subnode

- [[std-move-if-noexcept|std::move_if_noexcept]] — uses `noexcept` to choose between move and copy for exception-safe operations

> Full coverage: [[Chapter 27 — Exceptions]] → noexcept
