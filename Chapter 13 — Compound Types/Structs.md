---
tags:
  - cpp/types
  - cpp/structs
  - concept
  - syntax
  - best-practice
aliases:
  - struct
  - aggregate
  - aggregate initialization
  - data member
  - member variable
  - memberwise initialization
  - designated initializer
  - default member initializer
  - padding
  - struct padding
up: "[[Chapter 13 — Compound Types]]"
related:
  - "[[Program-defined Types]]"
  - "[[Class Templates]]"
  - "[[Initialization]]"
  - "[[List Initialization]]"
  - "[[Pass by Reference]]"
  - "[[Pointers]]"
  - "[[Memory Model]]"
  - "[[One Definition Rule]]"
  - "[[Header Files]]"
---

# Structs

A **struct** is a class type (alongside `class` and `union`). It groups related variables — called **data members** (or **member variables**) — under a single name.

```cpp
struct Employee
{
    int id {};
    int age {};
    double wage {};
};
```

Members are accessed with the **member selection operator** (`.`):

```cpp
Employee joe {};
joe.age = 32;
std::cout << joe.age;
```

## Initialization

### Aggregate initialization

A struct without user-provided constructors is an **aggregate**. Aggregates support **aggregate initialization** via a braced initializer list:

```cpp
Employee joe { 2, 28, 45000.0 }; // memberwise initialization in declaration order
```

Members are initialized in declaration order (**memberwise initialization**). Prefer the non-copy braced form.

**Data members are not initialized by default** — always provide default values.

### Default member initializers

```cpp
struct Something
{
    int x;       // no default (bad — uninitialized)
    int y {};    // value-initialized to 0
    int z { 2 }; // default value 2
};
```

Explicit initialization values in the aggregate initializer **take precedence** over default member values.

### Designated initializers (C++20)

Initialize members by name — allows skipping members (which get their defaults):

```cpp
Foo f1{ .a{ 1 }, .c{ 3 } };   // .b gets its default
Foo f2{ .a = 1, .c = 3 };     // assignment-style
```

### Copy from another struct

```cpp
Foo foo { 1, 2, 3 };
Foo x = foo;  // copy-initialization
Foo y(foo);   // direct-initialization
Foo z {foo};  // direct-list-initialization
```

## Passing and returning structs

Pass structs **by const reference** to avoid copies (see [[Pass by Reference]]):

```cpp
void printEmployee(const Employee& employee)
{
    std::cout << "ID:   " << employee.id << '\n';
}
```

Pass a temporary struct directly at the call site:

```cpp
printEmployee(Employee { 14, 32, 24.15 });
```

Return structs by value — the compiler can return a nameless temporary:

```cpp
return { 0.0, 0.0, 0.0 }; // unnamed Point3d
return {};                  // value-initialized Point3d
```

## Member selection via pointer

Use the arrow operator (`->`) when accessing a struct through a pointer (see [[Pointers]]):

```cpp
Employee* ptr { &joe };
std::cout << ptr->id; // equivalent to (*ptr).id
```

`operator->` can be chained.

## Ownership and layout

Prefer struct data members to be **owning types** (not pointers or references) so the struct owns its data.

The struct's size is *at least* the sum of its members. The compiler may insert **padding** bytes between members for alignment, making the actual size larger (see [[Memory Model]]).

> Full coverage: [[Chapter 13 — Compound Types]] → Structs
