---
tags:
  - cpp/functions
  - concept
  - best-practice
aliases:
  - recursive function
  - tail call
  - tail recursion
  - memoization
  - base case
up: "[[Chapter 20 — Functions]]"
related:
  - "[[The Stack and the Heap]]"
  - "[[Functions]]"
  - "[[Chapter 20 — Functions]]"
---

# Recursion

A **recursive function** is one that calls itself. Every recursive function needs a **base case** — a condition that terminates the recursion — otherwise it runs until it causes a stack overflow (see [[The Stack and the Heap]]).

```cpp
int factorial(int n)
{
    if (n <= 1) return 1;   // base case
    return n * factorial(n - 1);
}
```

## Tail calls

A **tail call** is a function call that occurs at the very end of a function, with its return value directly returned by the caller. Tail-recursive functions are easy for compilers to optimize into iterative loops (tail-call optimization / TCO), eliminating the stack frame growth:

```cpp
int factorial(int n, int acc = 1)
{
    if (n <= 1) return acc;
    return factorial(n - 1, n * acc); // tail call
}
```

## Memoization

**Memoization** caches the results of expensive sub-calls so that repeated inputs return immediately rather than recomputing:

```cpp
int fib(int n, std::unordered_map<int,int>& cache)
{
    if (n <= 1) return n;
    if (cache.count(n)) return cache[n];
    return cache[n] = fib(n-1, cache) + fib(n-2, cache);
}
```

Without memoization, naive Fibonacci is exponential; with it, each value is computed once.

## When to use recursion

Prefer recursion when the problem is naturally hierarchical (trees, graphs, divide-and-conquer). Prefer iteration when the problem is linear and recursion depth could be large — iterative solutions avoid stack pressure and are generally faster due to no function-call overhead.

> Full coverage: [[Chapter 20 — Functions]] → Recursion
