# Chapter F — Constexpr functions

Pin: No
Fav: No
Category: Done (https://www.notion.so/Done-24fb13b569b5836bb160816e5fa04870?pvs=21)
Created: April 13, 2026 8:45 PM
Last edited: May 14, 2026 3:27 PM

## Notes

- **Constexpr functions**
    - A **constexpr function** is a function that is allowed to be called in a constant expression.
    - To evaluate at compile-time, two other things must also be true:
        - The call to the constexpr function must have arguments that are known at compile time (e.g. are constant expressions).
        - All statements and expressions within the constexpr function must be evaluatable at compile-time.
    - When a constexpr function evaluates at runtime, it evaluates just like a normal (non-constexpr) function would. In other words, the `constexpr` has no effect in this case.
    - Compile-time evaluation of constexpr functions is only guaranteed when a constant expression is required.
    - All constexpr functions should be evaluatable at compile-time, as they will be required to do so in contexts that require a constant expression.
    - Always test your constexpr functions in a context that requires a constant expression, as the constexpr function may work when evaluated at runtime but fail when evaluated at compile-time.
    - **Constexpr/consteval function parameters are not constexpr**
    - **Constexpr functions are implicitly inline**
        - The compiler must be able to see the full definition of a constexpr (or consteval) function, not just a forward declaration.
        - Constexpr/consteval functions used in a single source file (.cpp) should be defined in the source file above where they are used.
        - Constexpr/consteval functions used in multiple source files should be defined in a header file so they can be included into each source file.
        - The actual requirement for the forward declaration of constexpr functions that are evaluated at compile-time is that “the constexpr function must be defined prior to the outermost evaluation that eventually results in the invocation”.
- **Consteval**
    - C++20 introduces the keyword **consteval**, which is used to indicate that a function *must* evaluate at compile-time, otherwise a compile error will result.
        - **Immediate functions**
    - Use `consteval` if you have a function that must evaluate at compile-time for some reason (e.g. because it does something that can only be done at compile time).
    - `std::is_constant_evaluated()` (defined in the <type_traits> header) returns a `bool` indicating whether the current function is executing in a constant-evaluated context.
        - A **constant-evaluated context** (also called a **constant context**) is defined as one in which a constant expression is required (such as the initialization of a constexpr variable).
        - Introduced in C++23, `if consteval` is a replacement for `if (std::is_constant_evaluated())` that provides a nicer syntax and fixes some other issues.
- **Constexpr/consteval functions can use non-const local variables**
- **Constexpr/consteval functions can use function parameters and local variables as arguments in constexpr function calls**

```cpp
constexpr int goo(int c) // goo() is now constexpr
{
    return c;
}

constexpr int foo(int b) // b is not a constant expression within foo()
{
    return goo(b);       // if foo() is resolved at compile-time, then `goo(b)` can also be resolved at compile-time
}
```

- A **pure function** is a function that meets the following criteria:
    - The function always returns the same return result when given the same arguments
    - The function has no side effects (e.g. it doesn’t change the value of static local or global variables, doesn’t do input or output, etc…).
    - Pure functions should generally be made constexpr.
-