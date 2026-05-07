---
tags:
  - cpp/scope
  - cpp/variables
  - concept
  - best-practice
  - subnode
aliases:
  - name hiding
  - variable hiding
up: "[[Block Scope]]"
related:
  - "[[Block Scope]]"
  - "[[Global Variables]]"
  - "[[Local Variables]]"
  - "[[Undefined Behavior]]"
---

# Variable Shadowing

**Variable shadowing** (also called **name hiding**) occurs when a variable in an inner scope has the same name as a variable in an outer scope. The inner variable "shadows" the outer one, making the outer inaccessible within that inner scope.

## Nested block shadowing

```cpp
int x { 1 };         // outer x
{
    int x { 2 };     // shadows outer x
    // outer x is inaccessible here
}
// outer x is accessible again
```

## Local variable shadowing a global

A local variable with the same name as a global variable shadows the global within the function:

```cpp
int g_value { 5 };   // global

void foo()
{
    int g_value { 10 };  // shadows global g_value
}
```

## Best practice

**Avoid variable shadowing.** It introduces confusion about which variable is being modified and can hide bugs — in subtle cases it can contribute to [[Undefined Behavior]].

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Variable Shadowing
