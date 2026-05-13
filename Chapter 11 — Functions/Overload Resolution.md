---
tags:
  - cpp/functions
  - concept
  - syntax
  - subnode
aliases:
  - overload resolution
  - ambiguous match
  - trivial conversion
up: "[[Function Overloading]]"
related:
  - "[[Function Overloading]]"
  - "[[Deleted Functions]]"
  - "[[Default Arguments]]"
  - "[[Numeric Promotion]]"
  - "[[Numeric Conversions]]"
  - "[[Implicit Type Conversion]]"
  - "[[Chapter 11 — Functions]]"
---

# Overload Resolution

When a function call targets an overloaded function, the compiler steps through a priority sequence of matching rules to find the **best** overload. Each step applies progressively broader type conversions.

## Matching steps (in order)

1. **Exact match** — argument type already matches the parameter type, possibly after *trivial conversions* (type modifications that don't change the value, e.g. `T[]` → `T*`, `T` → `const T`).
2. **[[Numeric Promotion]]** — e.g. `char`/`short` → `int`, `float` → `double`.
3. **[[Numeric Conversions]]** — any standard integral or floating-point conversion not covered by promotion.
4. **User-defined conversions** — a class `operator T()` or a converting constructor:

   ```cpp
   class X {
   public:
       operator int() { return 0; } // user-defined conversion X → int
   };
   ```

5. **Ellipsis** — `...` parameter is a last-resort catch-all.

The compiler stops at the first step that produces a match.

## Multi-argument calls

When the call has multiple arguments, the compiler applies the rules to each argument independently. The selected overload must be at least as good as every other candidate for every parameter, and strictly better for at least one.

## Ambiguous matches

An **ambiguous match** occurs when two or more overloads tie at the same step:

```cpp
void foo(int x);
void foo(double x);

foo(5L); // long → int and long → double are both numeric conversions → ambiguous
```

The compiler issues a compile error. Resolving ambiguity: explicitly cast the argument, or add an overload for the exact type.

[[Default Arguments]] can also create ambiguous matches when a single-argument call could satisfy multiple overloads whose extra parameters are all defaulted.

> Full coverage: [[Chapter 11 — Functions]] → Function Overloading
