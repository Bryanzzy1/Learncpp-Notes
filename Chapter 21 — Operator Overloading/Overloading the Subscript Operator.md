---
tags:
  - cpp/operators
  - cpp/classes
  - concept
  - syntax
  - best-practice
aliases:
  - operator[] overload
  - subscript operator overload
  - overload operator[]
up: "[[Chapter 21 — Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Member Function Overloads]]"
  - "[[Const Member Functions]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Overloading the Subscript Operator

`operator[]` must be overloaded as a **member function** (the language requires it). It returns a reference so it can appear on both sides of an assignment.

## Basic implementation

```cpp
int& operator[](int index)
{
    return m_list[index];
}
```

Returning by reference allows `obj[i] = value` — the caller gets direct access to the element.

## Const and non-const versions

Provide two overloads — one for mutable objects (returns a mutable reference) and one for `const` objects (returns a `const` reference):

```cpp
int& operator[](int index)             // for non-const objects
{
    return m_list[index];
}

const int& operator[](int index) const // for const objects — read-only
{
    return m_list[index];
}
```

See [[Const Member Functions]] for how `const` at the end of a member function signature works.

### C++23 explicit object parameter

C++23 allows a single implementation using a deduced `this` parameter, eliminating duplication:

```cpp
auto&& operator[](this auto&& self, int index)
{
    return self.m_list[index]; // const-ness of self is deduced from call context
}
```

## Index validation

Add bounds checking to catch out-of-range access rather than allowing [[Undefined Behavior]]:

```cpp
int& operator[](int index)
{
    assert(index >= 0 && index < size());
    return m_list[index];
}
```

## Caveats

- **Pointers to objects don't mix with `operator[]`**: `ptr[i]` on a pointer to a class applies pointer arithmetic, not `operator[]`.
- **The parameter type is not restricted to integers**: you can overload `operator[]` with `std::string`, an enum, or any other type as the index.
- **C++23** adds support for multiple subscript parameters: `operator[](int row, int col)`.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Overloading the Subscript Operator
