---
tags:
  - cpp/control-flow
  - concept
  - syntax
aliases:
  - goto
  - statement label
  - unconditional jump
  - function scope
up: "[[Conditional Statements]]"
related:
  - "[[Conditional Statements]]"
  - "[[Control Flow]]"
  - "[[Loops]]"
  - "[[Functions]]"
  - "[[Undefined Behavior]]"
  - "[[Initialization]]"
---

# Goto Statements

A **goto statement** is an unconditional jump — it transfers execution directly to a **statement label** elsewhere in the same function.

```cpp
int main()
{
    double x{};
tryAgain: // statement label
    std::cout << "Enter a non-negative number: ";
    std::cin >> x;

    if (x < 0.0)
        goto tryAgain; // jump back to label

    std::cout << "The square root of " << x << " is " << std::sqrt(x) << '\n';
    return 0;
}
```

## Scope of statement labels

Statement labels have **function scope**: they are visible throughout the entire function, even before their point of declaration. This is different from block scope used by variables — see [[Functions]].

## Restrictions

- The `goto` and its target label must be in the **same function** — you cannot jump between functions.
- `goto` can jump forward or backward.
- **You cannot jump forward over the initialization of a variable that is still in scope at the destination.** Doing so would leave the variable uninitialized, which is [[Undefined Behavior]]. See [[Initialization]].
- You *can* jump backward over a variable initialization — the variable will be re-initialized each time the initialization statement is executed again.

## When to use

`goto` is generally avoided in modern C++ in favor of structured loops ([[Loops]]) and conditionals. Its main legitimate use is breaking out of deeply nested loops, though even then `break` or a flag variable is usually preferred.

> Full coverage: [[Chapter 8 — Control Flow]] → Goto Statements
