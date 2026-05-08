---
tags:
  - cpp/control-flow
  - cpp/loops
  - concept
  - syntax
  - subnode
aliases:
  - do-while
  - do while loop
  - do while statement
up: "[[Loops]]"
related:
  - "[[Loops]]"
  - "[[For Loop]]"
---

# Do-While Loop

A **do-while loop** executes its body **at least once**, then checks the condition. This is the key distinction from a `while` loop, which checks the condition first.

```cpp
do
    statement; // can be a single statement or a compound statement
while (condition);
```

```cpp
do
{
    std::cout << "Enter a number between 0 and 9: ";
    std::cin >> x;
} while (x < 0 || x > 9);
```

The body executes, then `condition` is evaluated. If `true`, the body runs again; if `false`, the loop exits.

## When to use

Use a do-while when the body must run before the condition is meaningful — for example, prompting for user input that will then be validated. In all other cases, prefer `while` over `do-while` for consistency and clarity.

> Full coverage: [[Chapter 8 — Control Flow]] → Do-While Loop
