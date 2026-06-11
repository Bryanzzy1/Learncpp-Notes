---
tags:
  - cpp/functions
  - cpp/pointers
  - concept
  - syntax
aliases:
  - function pointer
  - callback function
  - callback
  - fcnPtr
  - std::function
up: "[[Chapter 20 — Functions]]"
related:
  - "[[Lambdas]]"
  - "[[Default Arguments]]"
  - "[[Functions]]"
  - "[[Type Deduction]]"
  - "[[Void Pointers]]"
  - "[[Chapter 20 — Functions]]"
---

# Function Pointers

A **function pointer** is a variable that stores the address of a function and can be used to call that function indirectly.

## Declaration syntax

```cpp
int (*fcnPtr)(int);        // pointer to a function taking int, returning int
double (*gooPtr)();        // pointer to a function taking nothing, returning double
```

The return type and parameter types must match the pointed-to function exactly:

```cpp
int foo();
double goo();
int hoo(int x);

int (*fcnPtr1)(){ &foo };    // okay
int (*fcnPtr2)(){ &goo };    // wrong — return types don't match
double (*fcnPtr4)(){ &goo }; // okay
fcnPtr1 = &hoo;              // wrong — parameter mismatch
int (*fcnPtr3)(int){ &hoo }; // okay
```

## Implicit conversion and nullptr

- C++ implicitly converts a function to a function pointer when needed (the `&` is optional).
- Function pointers do **not** convert to [[Void Pointers|void pointers]], or vice-versa.
- A function pointer can be assigned `nullptr`.

## Calling through a pointer

```cpp
int (*fcnPtr)(int){ &foo };
fcnPtr(5);    // implicit dereference — preferred
(*fcnPtr)(5); // explicit dereference — also valid
```

## Default arguments don't work through function pointers

Default argument resolution happens at compile time using the declaration; a call through a function pointer is resolved at runtime, so defaults are ignored. This also means a function pointer can disambiguate calls that would be ambiguous due to overlapping defaults — see [[Default Arguments]].

## Callback functions

A function passed as an argument to another function is called a **callback function**:

```cpp
void selectionSort(int* array, int size, bool (*comparisonFcn)(int, int) = ascending);
```

## std::function

`std::function` (from `<functional>`) is a type-safe wrapper for any callable — function pointers, lambdas, or functors:

```cpp
#include <functional>
bool validate(int x, int y, std::function<bool(int, int)> fcn);
```

Prefer `std::function` over raw function pointers when storing or passing callables, unless performance is critical.

## Type inference

`auto` can infer a function pointer type:

```cpp
auto fcnPtr{ &foo }; // type deduced as int(*)()
```

For [[Lambdas]], `auto` is the only way to capture the lambda's exact (unnamed) type.

> Full coverage: [[Chapter 20 — Functions]] → Function Pointers
