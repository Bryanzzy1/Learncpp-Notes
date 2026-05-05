---
tags:
  - cpp/operators
  - cpp/arithmetic
  - cpp/comparison
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Operators
  - Ch6
up: LearnCPP
related:
  - "[[Arithmetic Operators]]"
  - "[[Increment and Decrement]]"
  - "[[Comma Operator]]"
  - "[[Conditional Operator]]"
  - "[[Relational Operators]]"
---

# Chapter 6 — Operators

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 5, 2026 3:36 PM

## Notes

- **[[Arithmetic Operators]]**
    - **% is Remainder**
        - 21 modulo 4 = 3
        - 21 remainder 4 = -1 (C++)
    - **Exponents**
        - To do exponents in C++, #include the <cmath> header, and use the pow() function:
            
            ```cpp
            #include <cmath>
            
            double x{ std::pow(3.0, 4.0) }; // 3 to the 4th power
            ```
            
- **[[Increment and Decrement|Increment/decrement operators]]**
    
    | **Operator** | **Symbol** | **Form** | **Operation** |
    | --- | --- | --- | --- |
    | Prefix increment (pre-increment) | ++ | ++x | Increment x, then return x |
    | Prefix decrement (pre-decrement) | –– | ––x | Decrement x, then return x |
    | Postfix increment (post-increment) | ++ | x++ | Copy x, then increment x, then return the copy |
    | Postfix decrement (post-decrement) | –– | x–– | Copy x, then decrement x, then return the copy |
    - A function or expression is said to have a **side effect** if it has some observable effect beyond producing a return value.
    - NOTE: The C++ standard does not define the order in which function arguments are evaluated.
    - C++ also does not specify when the side effects of operators must be applied.
    - For example, the expression `x + ++x` is unspecified behavior. When `x` is initialized to `1`, Visual Studio and GCC evaluate this as `2 + 2`, and Clang evaluates it as `1 + 2`! This is due to differences in when the compilers apply the side effect of incrementing `x`.
- **[[Comma Operator]]**
    - Evaluate x, then y, returns the value of y
    - `std::cout << (++x, ++y) << '\n'; // increment x and y, evaluates to the right operand`
- **[[Conditional Operator]]**
    - ?:
    - c ? x : y
        - If conditional `c` is `true` then evaluate `x`, otherwise evaluate `y`
    - Parenthesize the entire conditional operation (including operands) when used in a compound expression.
    - To comply with C++'s type checking rules, one of the following must be true:
        - The type of the second and third operands must match.
        - The compiler must be able to find a way to convert one or both of the second and third operands to matching types. The conversion rules the compiler uses are fairly complex and may yield surprising results in some cases.
        - Alternatively, one or both of the second and third operands is allowed to be a throw expression.
- **[[Relational Operators]]**
    - Avoid using operator== and operator!= to compare floating point values if there is any chance that those values have been calculated.
        - NOTE: It is safe to compare a floating-point literal with a variable of the same type that has been initialized with a literal of the same type
