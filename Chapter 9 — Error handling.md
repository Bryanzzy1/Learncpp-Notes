# Chapter 9 — Error handling

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 9, 2026 3:41 PM

## Notes

- Write your program in small, well defined units (functions or classes), compile often, and test your code as you go.
- **Informal testing**
    - you can write some code to test the unit that was just added, and then erase the test once the test passes.
    - It can make more sense to preserve your tests so they can be run again in the future.
- A better method is to use `assert`, which will cause the program to abort with an error message if any test fails.
    - `#include <cassert> // for assert()`
    - `#define NDEBUG` (to disable asserts) or `#undef NDEBUG` (to enable asserts).
    - **static_assert**
        - An assertion that is checked at compile-time rather than at runtime, with a failing `static_assert` causing a compile error.
        - The condition must be a constant expression.
        - Favor `static_assert` over `assert()` whenever possible.
- **Code coverage**
    - Used to describe how much of the source code of a program is executed while testing.
- **Statement coverage**
    - The percentage of statements in your code that have been exercised by your testing routines.
- **Branch coverage**
    - The percentage of branches that have been executed, each possible branch counted separately.
    - Aim for 100% branch coverage of your code.
- **Loop coverage**
    - (informally called **the 0, 1, 2 test**) says that if you have a loop in your code, you should ensure it works properly when it iterates 0 times, 1 time, and 2 times.
    - If it works correctly for the 2-iteration case, it should work correctly for all iterations greater than 2.
- Test different categories of input values to make sure your unit handles them properly.
- Defined **defensive programming** as the practice of trying to anticipate all of the ways software can be misused, either by end-users, or by developers (either the programmer themselves, or others).
- **Handling errors in functions**
    - Handling the error within the function
    - Passing errors back to the caller
        - A **sentinel value** is a value that has some special meaning in the context of a function or algorithm.
    - Fatal errors
        - If the error is so bad that the program can not continue to operate properly, this is called a **non-recoverable** error (also called a **fatal error**).
    - Exceptions
    -