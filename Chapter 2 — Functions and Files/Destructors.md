---
tags:
  - cpp/functions
  - cpp/memory
  - concept
  - subnode
aliases:
  - destructor
  - object destruction
  - object lifetime
up: "[[Functions]]"
related:
  - "[[Memory Model]]"
  - "[[Initialization]]"
  - "[[Pass by Value]]"
---

# Destructors

A **destructor** is a special member function that runs automatically just before a class-type object is destroyed.

**Destruction sequence:**
1. Destructor runs (performs cleanup — releasing resources, closing files, etc.)
2. The object's memory is deallocated and freed for reuse

For non-class types (`int`, `double`, pointers), there is no destructor — the memory is simply freed.

**Destruction order:**
Local variables are destroyed in **reverse order of creation** when the function exits:

```cpp
void foo() {
    Widget a;   // created first
    Widget b;   // created second
}               // b's destructor runs first, then a's
```

**Cost:** class types with trivial destructors (no user-defined cleanup needed) pay zero runtime cost — the compiler optimizes the destructor away entirely.

> Full coverage: [[Chapter 2 — Functions and Files]] → Local Scopes
