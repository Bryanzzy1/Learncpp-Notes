---
tags:
  - cpp/headers
  - cpp/files
  - best-practice
  - concept
aliases:
  - header
  - header guard
  - include
  - pragma once
  - translation unit
up: "[[Chapter 2 — Functions and Files]]"
related:
  - "[[Preprocessing]]"
  - "[[Functions]]"
  - "[[Namespaces]]"
---

# Header Files

Header files hold declarations so multiple `.cpp` files can share them without violating the One Definition Rule.

**Include resolution:**
- Angled brackets `<iostream>` — search system include directories (standard library / third-party)
- Double quotes `"myHeader.h"` — search the current directory first, then include directories

**Include order (preferred):**
1. Paired header for this source file (e.g. `add.h` for `add.cpp`)
2. Other project headers
3. Third-party library headers
4. Standard library headers

**Header guards** prevent a header from being processed more than once per translation unit:

```cpp
#ifndef MY_HEADER_H
#define MY_HEADER_H
// declarations here
#endif
```

**`#pragma once`** serves the same purpose with a single line, but may not deduplicate physically copied headers the way guards do.

- Each `.cpp` file should `#include` every header it needs — do not rely on transitively included headers.
- Use standard library headers without the `.h` extension; user-defined headers should keep `.h`.

> Full coverage: [[Chapter 2 — Functions and Files]] → Header Files
