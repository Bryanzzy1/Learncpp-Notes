---
tags:
  - cpp/control-flow
  - cpp/loops
  - concept
  - syntax
  - subnode
aliases:
  - for loop
  - for statement
  - multiple counters
  - ranged-for
up: "[[Loops]]"
related:
  - "[[Loops]]"
  - "[[Do-While Loop]]"
  - "[[Initialization]]"
---

# For Loop

A `for` loop packs initialization, condition, and increment into a single header, making it the idiomatic choice when the iteration count or index is known.

```cpp
for (init-statement; condition; end-expression)
    statement;
```

- **init-statement**: executed once before the loop begins (typically declares and initializes a counter).
- **condition**: evaluated before each iteration; loop exits when `false`.
- **end-expression**: executed at the end of each iteration (typically increments the counter).

Any of the three parts may be omitted (a missing condition is treated as `true`).

## Multiple counters

You can declare and update multiple variables in one `for` header using the comma operator:

```cpp
for (int x{ 0 }, y{ 9 }; x < 10; ++x, --y)
    std::cout << x << ' ' << y << '\n';
```

Both `x` and `y` must be declared with the same type in the init-statement. See [[Initialization]] for brace-initialization rules.

## Loop variable scope

Variables declared in the init-statement exist only for the duration of the loop. This limits their scope and avoids name collisions with outer code.

> Full coverage: [[Chapter 8 — Control Flow]] → For Loop
