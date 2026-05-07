---
tags:
  - cpp/linkage
  - concept
  - syntax
aliases:
  - internal linkage
  - internal variables
  - no linkage
  - static keyword
  - storage class specifier
up: "[[Chapter 7 — Scope, duration, and linkage]]"
related:
  - "[[External Linkage]]"
  - "[[Global Variables]]"
  - "[[Unnamed and Inline Namespaces]]"
  - "[[Storage Class Specifiers]]"
  - "[[Translation Unit]]"
  - "[[One Definition Rule]]"
---

# Internal Linkage

## Linkage levels

| Linkage | Who can see it |
|---------|---------------|
| No linkage | Only within its own block (local variables) |
| Internal linkage | Anywhere within the same [[Translation Unit]] |
| External linkage | Across all translation units |

## Local variables

Local variables have **no linkage** — each declaration is entirely independent.

## Internal linkage for globals

An identifier with **internal linkage** is visible within a single [[Translation Unit]] but inaccessible from other files. Two source files can each have an identically named internal identifier without conflict — unlike external linkage identifiers which must satisfy the [[One Definition Rule]] across the whole program.

Global variables with internal linkage are called **internal variables**.

### Making a global variable internal

```cpp
static int g_x;        // non-const global with internal linkage
```

Using `static` here is a **storage class specifier** — it sets both the name's linkage (internal) and storage duration (static). See [[Storage Class Specifiers]].

### Const and constexpr globals

`const` and `constexpr` global variables have **internal linkage by default** — no `static` needed.

## Unnamed namespaces

Unnamed namespaces are preferred when you need to give internal linkage to a wider range of identifiers (e.g., type definitions), not just variables and functions. See [[Unnamed and Inline Namespaces]].

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Internal Linkage
