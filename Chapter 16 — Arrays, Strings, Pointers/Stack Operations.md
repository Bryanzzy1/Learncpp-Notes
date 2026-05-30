---
tags:
  - cpp/vectors
  - cpp/containers
  - concept
  - syntax
  - subnode
aliases:
  - push_back
  - pop_back
  - emplace_back
  - vector stack
  - stack interface
up: "[[std-vector]]"
related:
  - "[[std-vector]]"
  - "[[Memory Model]]"
---

# Stack Operations

`std::vector` exposes a stack-like interface through four member functions:

| Function | Stack operation | Behavior |
|----------|----------------|----------|
| `push_back(val)` | Push | Appends `val` to the end; increments length |
| `pop_back()` | Pop | Removes the last element; returns `void` |
| `back()` | Top / Peek | Returns a reference to the last element; does not remove it |
| `emplace_back(args...)` | Push | Constructs a new element in-place at the end |

## push_back vs emplace_back

Both `push_back` and `emplace_back` increment the vector's length and trigger **reallocation** if the current capacity is insufficient.

`emplace_back` is preferred when constructing a temporary of the element type to immediately push:

```cpp
std::vector<std::string> words;
words.push_back(std::string("hello"));   // constructs temporary, then moves/copies
words.emplace_back("hello");             // constructs directly in-place — more efficient
```

For trivial types (like `int`), the difference is negligible; the benefit shows with types that have non-trivial constructors.

## resize vs reserve

These are distinct operations that affect the vector differently:

```cpp
v.resize(n);    // sets length to n; initializes new elements; grows capacity if needed
v.reserve(n);   // sets capacity to at least n; does NOT change length or add elements
```

Use `reserve` before a known sequence of `push_back` calls to avoid repeated reallocations:

```cpp
std::vector<int> v;
v.reserve(1000);               // allocate once
for (int i = 0; i < 1000; ++i)
    v.push_back(i);            // no reallocation
```

> Full coverage: [[Chapter 16 — Arrays, Strings, Pointers]] → Stack Operations
