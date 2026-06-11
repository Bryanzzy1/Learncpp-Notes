---
tags:
  - cpp/functions
  - cpp/lambdas
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - capture clause
  - capture by value
  - capture by reference
  - default capture
  - mutable lambda
  - lambda capture
  - capture-default
up: "[[Lambdas]]"
related:
  - "[[Lambdas]]"
  - "[[Lvalue References]]"
  - "[[Undefined Behavior]]"
  - "[[std-reference-wrapper]]"
  - "[[Chapter 20 — Functions]]"
---

# Lambda Captures

The **capture clause** `[...]` of a [[Lambdas|lambda]] controls which variables from the enclosing scope the lambda can access. Unlike a nested block — where all outer-scope identifiers are visible — a lambda only sees what it explicitly captures.

## Capture by value

A captured variable is **cloned** into the lambda at the point the lambda is defined. Captures are `const` by default.

```cpp
int search = 5;
auto fn = [search](int x) { return x == search; }; // search is a copy
```

### Mutable captures

To modify a by-value capture, mark the lambda `mutable`:

```cpp
auto shoot{
    [ammo]() mutable {
        --ammo; // modifies the clone, not the original
        std::cout << ammo << " shots left.\n";
    }
};
```

`mutable` applies to the entire lambda's `operator()`, not individual captures.

## Capture by reference

Prefix the variable name with `&` to capture a reference to the original:

```cpp
auto shoot{
    [&ammo]() {
        --ammo; // modifies the original
        std::cout << ammo << " shots left.\n";
    }
};
```

- By-reference captures are **non-const** unless the captured variable itself is `const`.
- Prefer capture by reference for non-fundamental types, just as you would prefer passing by [[Lvalue References|reference]] to a function.

## Default captures

A **default capture** (`=` or `&`) automatically captures every variable mentioned in the lambda body:

```cpp
[=]()  { /* all used variables by value    */ };
[&]()  { /* all used variables by reference */ };
```

Defaults can be mixed with explicit overrides — each variable may only be captured once:

```cpp
[health, armor, &enemies](){};   // armor/health by value, enemies by reference
[=, &enemies](){};               // everything by value except enemies
[&, armor](){};                  // everything by reference except armor

[&, &armor]{};  // illegal — armor captured twice by reference
[=, armor]{};   // illegal — armor captured twice by value
```

## Defining new variables in the capture

Introduce a new variable visible only inside the lambda:

```cpp
[userArea{ width * height }](int knownArea) {
    return userArea == knownArea;
};
```

Only do this when the initializer is short and the type is obvious; otherwise define the variable outside the lambda and capture it normally.

## Dangling captured variables

If a variable captured **by reference** goes out of scope before the lambda executes, the lambda holds a dangling reference — [[Undefined Behavior]]. This is especially dangerous with lambdas stored in `std::function` or passed to asynchronous code.

## Unintended copies of mutable lambdas

Because lambdas are objects, they can be copied. A copied mutable lambda has its own independent copy of captured state:

```cpp
auto count = [n = 0]() mutable { return ++n; };
auto copy = count; // independent copy of n

count(); // n = 1
copy();  // copy's n = 1 (not 2!)
```

To prevent inadvertent copies, wrap the lambda in `std::function` immediately and pass by reference, or use [[std-reference-wrapper|`std::ref`]]:

```cpp
// Pass mutable lambda by reference using std::ref
myInvoke(std::ref(myLambda));
```

Standard library algorithms may copy their callable argument — always use `std::ref` when passing a mutable lambda to them.

**Best practice:** avoid mutable lambdas when possible. Non-mutable lambdas are easier to reason about and safe to copy.

> Full coverage: [[Chapter 20 — Functions]] → Lambdas → Lambda Captures
