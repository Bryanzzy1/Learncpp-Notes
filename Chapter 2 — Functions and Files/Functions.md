---
tags:
  - cpp/functions
  - cpp/scope
  - concept
  - syntax
aliases:
  - function
  - local scope
  - forward declaration
  - function definition
  - destructor
up: "[[Chapter 2 — Functions and Files|Chapter 2 — Functions and Files]]"
related:
  - "[[Pass by Value]]"
  - "[[Memory Model]]"
  - "[[Preprocessing]]"
  - "[[Debugging]]"
  - "[[Destructors]]"
  - "[[Return by Value]]"
  - "[[One Definition Rule]]"
---

# Functions

A named block of reusable code that optionally takes parameters and returns a value.

```cpp
int add(int x, int y) {   // return type | name | parameters
    return x + y;
}
```

**`main()`:**
- Required entry point; must return `int`
- Implicitly returns `0` if no return statement is present
- Cannot be called explicitly

**Arguments and parameters:**
- Arguments are [[Pass by Value|passed by value]] — each parameter is a copy of its argument
- An **unnamed parameter** (`void foo(int)`) suppresses unused-parameter warnings

**Local scope:**
- Local variables are created on function entry and destroyed in reverse order on function exit
- For class-type objects, a **[[Destructors|destructor]]** runs before the variable is destroyed
- After destruction, the [[Memory Model|memory]] is deallocated and freed for reuse

**Forward declaration:**
- Tells the compiler a function exists before its definition appears in the file
- Allows functions to be defined in any order across multiple files
- **[[One Definition Rule]]**: a function may be declared many times but defined only once

> Full coverage: [[Chapter 2 — Functions and Files|Chapter 2 — Functions and Files]]
