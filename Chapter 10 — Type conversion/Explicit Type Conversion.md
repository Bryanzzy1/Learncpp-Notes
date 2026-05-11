---
tags:
  - cpp/types
  - cpp/casting
  - concept
  - syntax
  - best-practice
aliases:
  - explicit conversion
  - casting
  - dynamic_cast
  - const_cast
  - reinterpret_cast
  - C-style cast
  - function-style cast
up: "[[Chapter 10 — Type conversion]]"
related:
  - "[[Narrowing Conversions]]"
  - "[[Implicit Type Conversion]]"
  - "[[static_cast]]"
  - "[[Implicit Conversion]]"
  - "[[List Initialization]]"
---

# Explicit Type Conversion

**Explicit type conversion** (casting) is when the programmer instructs the compiler to convert a value to a different type using a cast operator.

## Cast operators

| Cast | Description | Safe? |
| --- | --- | --- |
| `static_cast` | Compile-time conversion between related types | Yes |
| `dynamic_cast` | Runtime conversion on pointers/references in a polymorphic hierarchy | Yes |
| `const_cast` | Add or remove `const` | Only for adding |
| `reinterpret_cast` | Reinterpret the bit pattern as another type | No |
| C-style cast | Some combination of the above | No |

Avoid `const_cast` and `reinterpret_cast` — they are only useful in rare cases and can be harmful if misused.

## static_cast

The preferred cast for most explicit conversions. Evaluated at compile time.

```cpp
static_cast<int>(c)        // prints numeric value instead of char glyph
static_cast<int>(3.5)      // truncates: → 3
```

Use `static_cast` when you need a [[Narrowing Conversions|narrowing conversion]] and want to be explicit about it. See [[static_cast]] for Ch4-level details.

### Two ways to convert to int

```cpp
static_cast<int>(x)  // direct-initialized temporary int
int { x }            // direct-list-initialized temporary int (disallows narrowing)
```

The second form uses [[List Initialization]], so it rejects narrowing conversions at compile time.

## C-style casts

```cpp
(double)x / y    // C-style cast
double(x) / y    // function-style cast (equivalent)
```

C-style casts silently try `static_cast`, then `const_cast`, then `reinterpret_cast` in sequence — the exact behavior is not obvious from the syntax. Avoid them; prefer `static_cast` instead.

> Full coverage: [[Chapter 10 — Type conversion]] → Explicit Type Conversion
