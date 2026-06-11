---
tags:
  - cpp/operators
  - cpp/io
  - concept
  - syntax
  - best-practice
aliases:
  - overload operator<<
  - overload operator>>
  - output operator
  - input operator
  - stream operator overloading
  - transactional input
  - Overloading I/O Operators
up: "[[Chapter 21 — Operator Overloading]]"
related:
  - "[[Operator Overloading]]"
  - "[[Friend Function Overloads]]"
  - "[[Normal Function Overloads]]"
  - "[[Member Function Overloads]]"
  - "[[Pass by Reference]]"
  - "[[Chapter 21 — Operator Overloading]]"
---

# Overloading I/O Operators

`operator<<` and `operator>>` cannot be member functions because their left operand is `std::ostream`/`std::istream` — a type you do not own. They are implemented as [[Friend Function Overloads|friend]] or [[Normal Function Overloads|normal]] non-member functions.

## Overloading `operator<<` (output)

```cpp
std::ostream& operator<<(std::ostream& out, const Point& point)
{
    out << "Point(" << point.m_x << ", " << point.m_y << ", " << point.m_z << ')';
    return out; // return stream to allow chaining: std::cout << a << b
}
```

- First parameter: `std::ostream&` (the stream — `std::cout`, a file stream, etc.)
- Second parameter: the user-defined type, usually `const T&` (see [[Pass by Reference]])
- **Return `out`** to support chaining (`std::cout << a << b`).

## Overloading `operator>>` (input)

```cpp
std::istream& operator>>(std::istream& in, Point& point)
{
    in >> point.m_x >> point.m_y >> point.m_z;
    return in;
}
```

- The user-defined parameter must be **non-const** (you are modifying it).
- Return `in` for chaining.

## Guarding against partial extraction

A naive `operator>>` leaves the object half-modified if input fails midway — a violation of the **transactional** principle (an operation must either fully succeed or fully fail). Use local temporaries and assign only on full success:

```cpp
std::istream& operator>>(std::istream& in, Point& point)
{
    double x{}, y{}, z{};
    in >> x >> y >> z;
    point = in ? Point{x, y, z} : Point{}; // assign only if stream is good
    return in;
}
```

This non-friend version works if `Point` has a suitable constructor and assignment operator. Always validate semantic constraints (e.g. range checks) separately after extraction.

> Full coverage: [[Chapter 21 — Operator Overloading]] → Overloading IO Operators
