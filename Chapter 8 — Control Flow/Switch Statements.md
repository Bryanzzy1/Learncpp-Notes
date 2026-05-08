---
tags:
  - cpp/control-flow
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - switch statement
  - case label
  - default label
  - fallthrough
  - break statement
  - switch-case
up: "[[Conditional Statements]]"
related:
  - "[[Conditional Statements]]"
  - "[[Control Flow]]"
  - "[[Undefined Behavior]]"
  - "[[Initialization]]"
---

# Switch Statements

A `switch` statement evaluates an integral or enum expression against a set of constant labels, jumping directly to the matching one.

```cpp
switch (x)
{
case 1:
    std::cout << "One";
    break;
case 2:
    std::cout << "Two";
    break;
case 3:
    std::cout << "Three";
    break;
default:
    std::cout << "Unknown";
    break;
}
```

## Case labels

- The value after `case` must be a constant expression that matches or is convertible to the condition's type.
- All case labels in a switch must be **unique**.
- The `default` label is optional; there can be at most one per switch.
- If no label matches and there is no `default`, the entire switch is skipped.

## Break statement

A `break` exits the switch block, continuing execution after it. Without a `break` or `return`, execution **falls through** into the next case.

## Fallthrough

**Fallthrough** occurs when execution flows from one case's statements into the next case's statements. It is usually a bug; signal intentional fallthrough with the `[[fallthrough]]` attribute (C++17):

```cpp
switch (2)
{
case 1:
    std::cout << 1 << '\n';
    break;
case 2:
    std::cout << 2 << '\n';
    [[fallthrough]]; // intentional — note the semicolon (null statement)
case 3:
    std::cout << 3 << '\n'; // also executed when case 2 matches
    break;
}
```

`[[fallthrough]]` is a C++ **attribute** — a compiler hint that suppresses the fallthrough warning.

## Variable declarations inside switch

All statements inside a switch share the same scope (the switch block). You may **declare or define** (but not **initialize**) variables before the last case:

- Initialization is disallowed in any case that is not the last, because the switch may jump over the initializer, leaving the variable in an indeterminate state — reading it would be [[Undefined Behavior]].
- Wrap a case in its own block `{}` if you need a local variable with an initializer.

See [[Initialization]] for why uninitialized variables are dangerous.

> Full coverage: [[Chapter 8 — Control Flow]] → Switch Statements
