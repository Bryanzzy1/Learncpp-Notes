---
tags:
  - cpp/linkage
  - concept
  - syntax
  - best-practice
aliases:
  - external linkage
  - external variables
  - extern keyword
up: "[[Chapter 7 — Scope, duration, and linkage]]"
related:
  - "[[Internal Linkage]]"
  - "[[Global Variables]]"
  - "[[Forward Declaration Summary]]"
  - "[[Storage Class Specifiers]]"
  - "[[Functions]]"
  - "[[Translation Unit]]"
  - "[[One Definition Rule]]"
  - "[[Header Files]]"
---

# External Linkage

An identifier with **external linkage** can be seen and used both from the file in which it is defined and from other translation units (via a forward declaration).

## Functions

**[[Functions]] have external linkage by default.** No special keyword is needed to share a function across files — just provide a forward declaration in the other files (typically via a [[Header Files|header file]]). This is governed by the [[One Definition Rule]]: the function may be declared many times but must be defined exactly once.

## External global variables

Global variables with external linkage are called **external variables**.

- **Non-constant globals have external linkage by default.**
- To explicitly give a `const` global external linkage, use `extern`:

```cpp
// file1.cpp — definition
extern const int g_x { 1 };

// file2.cpp — forward declaration (no initializer)
extern const int g_x;
```

## Warning

**Avoid using `extern` on a non-const global variable with an initializer** — this conflates definition and declaration in a confusing way.

## How to share a global across files

1. Define the variable in one `.cpp` file.
2. Place a forward declaration (`extern`, no initializer) in every other file that needs it — or put the forward declaration in a [[Header Files|header file]] that each file includes.

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → External Linkage
