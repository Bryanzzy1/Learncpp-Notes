---
tags:
  - cpp/references
  - cpp/functions
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - pass by reference
  - pass by lvalue reference
  - pass by const reference
  - pass by const lvalue reference
up: "[[Lvalue References]]"
related:
  - "[[Lvalue References]]"
  - "[[Lvalue References to Const]]"
  - "[[Pass by Value]]"
  - "[[Pass by Address]]"
  - "[[Functions]]"
  - "[[Strings]]"
  - "[[Type Deduction with References and Pointers]]"
  - "[[Chapter 12 — References and Pointers]]"
---

# Pass by Reference

**Pass by reference** uses a reference-type function parameter so the function operates directly on the caller's object without copying it.

```cpp
void increment(int& x)         { ++x; }           // modifies caller's variable
void print(const std::string& s) { std::cout << s; } // reads without copying
```

## Pass by non-const reference

Declares the parameter as `T&`. The function **can modify** the caller's argument. Accepts **only modifiable lvalue** arguments — rvalues and const objects are rejected.

Use when the function intentionally needs to change the value of the argument.

## Pass by const reference

Declares the parameter as `const T&`. The function **cannot modify** the argument. Accepts modifiable lvalues, non-modifiable (const) lvalues, and rvalues — the broadest possible binding.

**Favor pass by const reference over pass by non-const reference** unless modification is required.

**Ensure type compatibility**: passing a value that requires implicit conversion through a const reference silently creates a temporary, which may be an unexpected (and expensive) copy.

## When to pass by value vs. by reference

| Argument type | Preferred method | Reason |
| --- | --- | --- |
| Fundamental types, enums | [[Pass by Value]] | Cheap to copy; compiler can optimize better |
| Class types (strings, vectors, etc.) | `const T&` | Copy may be expensive |
| `std::string_view` | By value | Already a lightweight view into a string |

Prefer `std::string_view` (by value) over `const std::string&` unless the function calls other code that requires a C-string or `std::string`. See [[Strings]].

## Performance details

- **Pass by value**: the compiler may place the copy in a CPU register; each access is a single memory operation.
- **Pass by reference**: each use requires two memory accesses — first read the reference to find the target address, then read/write the target.
- An object is considered **cheap to copy** if `sizeof(T) <= 2 * sizeof(void*)` and it has no setup costs.
- For cheap objects, pass by value is at least as fast and often enables better compiler optimizations.
- For expensive-to-copy objects, the copy cost dominates, making pass by reference clearly preferable.

> Full coverage: [[Chapter 12 — References and Pointers]] → Lvalue References
