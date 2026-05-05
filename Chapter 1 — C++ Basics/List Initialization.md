---
tags:
  - cpp/initialization
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - brace initialization
  - uniform initialization
  - list initialization
  - narrowing conversion
  - value initialization
  - zero initialization
up: "[[Initialization]]"
related:
  - "[[Initialization]]"
  - "[[Implicit Conversion]]"
  - "[[static_cast]]"
---

# List Initialization

**List initialization** (also called **brace initialization** or **uniform initialization**) is the modern, preferred way to initialize objects in C++.

```cpp
int d { 7 };    // direct-list-initialization
int e {};       // value-initialization — zero-initialized to 0
```

## Why It's Preferred: Narrowing Conversions

Brace initialization disallows **narrowing conversions** — the compiler rejects any initialization that would silently lose data:

```cpp
int w1 { 4.5 };   // compile error: double → int would truncate .5
int w2 = 4.5;     // compiles silently: w2 copy-initialized to 4
int w3 (4.5);     // compiles silently: w3 direct-initialized to 4
```

This makes bugs visible at compile time instead of hiding them as silent truncation.

## Value Initialization / Zero Initialization

- `int e {};` — the empty braces trigger **value-initialization**
- For scalar types (`int`, `double`, pointers): value-initialized to zero (`0`, `0.0`, `nullptr`)
- For class types: value-initialization may invoke a user-defined constructor or zero-initialize members

> Full coverage: [[Chapter 1 — C++ Basics]] → Initialization
