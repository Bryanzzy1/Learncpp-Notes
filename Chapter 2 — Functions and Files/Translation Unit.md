---
tags:
  - cpp/preprocessing
  - cpp/files
  - concept
  - subnode
aliases:
  - translation unit
  - separate compilation
  - compilation model
up: "[[Preprocessing]]"
related:
  - "[[Preprocessing]]"
  - "[[Header Files]]"
  - "[[One Definition Rule]]"
  - "[[Namespaces]]"
---

# Translation Unit

A **translation unit** is a single `.cpp` source file after the preprocessor has finished — all `#include` contents are expanded, all macros are substituted, and all conditional compilation blocks are resolved. It is what the compiler actually receives.

**Separate compilation model:**
1. Preprocessor runs on each `.cpp` file → produces a translation unit
2. Compiler processes each translation unit **independently** → produces an object file (`.obj` / `.o`)
3. Linker combines all object files → produces the final executable

```
main.cpp  →  [preprocessor]  →  translation unit  →  [compiler]  →  main.obj
add.cpp   →  [preprocessor]  →  translation unit  →  [compiler]  →  add.obj
                                                        ↓
                                               [linker] → program.exe
```

**Why it matters:**
- The [[One Definition Rule]] applies per-program: a function defined in two translation units is a linker error
- `#include` guards / `#pragma once` prevent a header from being processed twice within the same translation unit
- Declarations (in headers) can appear in many translation units; definitions must appear in exactly one

> Full coverage: [[Chapter 2 — Functions and Files]] → Preprocessing
