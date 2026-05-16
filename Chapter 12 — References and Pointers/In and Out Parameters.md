---
tags:
  - cpp/functions
  - cpp/references
  - cpp/pointers
  - concept
  - best-practice
  - subnode
aliases:
  - in parameter
  - out parameter
  - in-out parameter
  - output parameter
up: "[[Pointers]]"
related:
  - "[[Pointers]]"
  - "[[Pass by Address]]"
  - "[[Pass by Reference]]"
  - "[[Functions]]"
  - "[[std-optional|std::optional]]"
  - "[[Chapter 12 — References and Pointers]]"
---

# In and Out Parameters

Function parameters can be classified by their data-flow direction relative to the caller.

## In parameters

A parameter used **only to receive input** — the function reads it but writes nothing back. Typically declared as `const T&` or `const T*`:

```cpp
void print(const std::string& s); // s is an in parameter
```

## Out parameters

A parameter used **only to return information** to the caller — the function writes to it but the initial value is irrelevant. Typically a non-const reference or non-const pointer:

```cpp
void getResult(int& result); // result is an out parameter
```

Out parameters are often confusing: at the call site, the caller cannot tell the variable will be modified without reading the function declaration. Prefer returning values from [[Functions|functions]] when possible.

For functions that may fail to produce a value, consider returning [[std-optional|std::optional]] instead of using an out parameter.

## In-out parameters

A parameter that the function **both reads and writes** — the initial value matters, and the function may change it. Non-const reference or pointer.

> Full coverage: [[Chapter 12 — References and Pointers]] → Pointers
