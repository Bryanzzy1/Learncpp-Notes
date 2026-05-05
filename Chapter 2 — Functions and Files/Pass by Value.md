---
tags:
  - cpp/functions
  - concept
  - syntax
aliases:
  - value parameters
  - pass-by-value
  - function arguments
  - function parameters
up: "[[Chapter 2 — Functions and Files|Chapter 2 — Functions and Files]]"
related:
  - "[[Initialization]]"
  - "[[Functions]]"
  - "[[constexpr]]"
---

# Pass by Value

When a function is called, each argument is **copy-initialized** into its matching parameter — see [[Initialization]].

```cpp
void modify(int x) { x = 99; }  // x is a copy; caller's variable is unaffected

int val { 5 };
modify(val);   // val is still 5
```

- Mutations inside the function never affect the caller's original variable
- Because the copy is discarded on return, adding `const` to a value parameter is not meaningful to callers
- Function parameters are always **runtime constants**: their value is not known until the moment of the call — this is why they cannot be declared [[constexpr]]
- An **unnamed parameter** (`void foo(int)`) silences unused-parameter warnings without providing a name

> Full coverage: [[Chapter 2 — Functions and Files|Chapter 2 — Functions and Files]] → Arguments and Parameters
