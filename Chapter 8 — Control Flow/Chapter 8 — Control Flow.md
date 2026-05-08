---
tags:
  - cpp/control-flow
  - cpp/loops
  - cpp/random
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Control Flow
  - Ch8
up: LearnCPP
related:
  - "[[Control Flow]]"
  - "[[Conditional Statements]]"
  - "[[Constexpr If Statement]]"
  - "[[Switch Statements]]"
  - "[[Goto Statements]]"
  - "[[Loops]]"
  - "[[Do-While Loop]]"
  - "[[For Loop]]"
  - "[[Halts]]"
  - "[[Random Number Generation]]"
  - "[[Pseudo-random Number Generators]]"
  - "[[Mersenne Twister]]"
---

# Chapter 8 — Control Flow

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 8, 2026 3:54 PM

## Notes

- **[[Control Flow]]**
    - When a program is run, the CPU begins execution at the top of `main()`
    - The specific sequence of statements that the CPU executes is called the program's **execution path** (or **path**, for short).
        - Straight-line programs take the same path (execute the same statements in the same order) every time they are run.
        - When a control flow statement causes point of execution to change to a non-sequential statement, this is called **branching**.
        
        | **Category** | **Meaning** | **Implemented in C++ by** |
        | --- | --- | --- |
        | Conditional statements | Causes a sequence of code to execute only if some condition is met. | if, else, switch |
        | Jumps | Tells the CPU to start executing the statements at some other location. | goto, break, continue |
        | Function calls | Jump to some other location and back. | function calls, return |
        | Loops | Repeatedly execute some sequence of code zero or more times, until some condition is met. | while, do-while, for, ranged-for |
        | Halts | Terminate the program. | std::exit(), std::abort() |
        | Exceptions | A special kind of flow control structure designed for error handling. | try, throw, catch |
- **[[Conditional Statements]]**
    - To avoid such ambiguities when nesting if-statements, it is a good idea to explicitly enclose the inner if-statement within a block
    - A **null statement** is an expression statement that consists of just a semicolon
        
        ```cpp
        if (x > 10)
            ; // this is a null statement
        ```
        
        - In Python, the `pass` keyword serves as a null statement.
        - In C++, we can mimic `pass` by using the preprocessor
        
        ```cpp
        #define PASS
        
        void foo(int x, int y)
        {
            if (x > y)
                PASS;
            else
                PASS;
        }
        ```
        
    - D**angling else**
        - occurs when it is ambiguous which `if statement` an `else statement` is connected to.
        - `Dangling else` statements are matched up with the last unmatched `if statement` in the same block.
        - We trivially avoid `dangling else` statements by ensuring the body of an `if statement` is placed in a block.
    - **[[Constexpr If Statement|Constexpr if statements]]**
        - Normally, the conditional of an if-statement is evaluated at runtime.
        - C++17 introduces the **constexpr if statement**
            - If the constexpr conditional evaluates to `true`, the entire if-else will be replaced by the true-statement. If the constexpr conditional evaluates to `false`, the entire if-else will be replaced by the false-statement (if it exists) or nothing (if there is no else).
    - **[[Switch Statements|Switch statement basics]]**
        
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
        
        - If no matching value can be found and there is no default label, the switch is skipped.
        - The constant expression after the case label must either match the type of the condition or must be convertible to that type.
            - All case labels in a switch must be unique.
        - The default label is optional, and there can only be one default label per switch statement.
        - A **break statement** (declared using the `break` keyword) tells the compiler that we are done executing statements within the switch, and that execution should continue with the statement after the end of the switch block.
            - Without a `break` or `return`, execution will overflow into subsequent cases.
        - The statements after labels are all scoped to the switch block. No implicit blocks are created.
            - All statements inside the switch are considered to be part of the same scope.
        - When execution flows from a statement underneath a label into statements underneath a subsequent label, this is called **fallthrough**.
            - **The [[fallthrough]] attribute**
                - **Attributes** are a modern C++ feature that allows the programmer to provide the compiler with some additional data about the code.
                - The `[[fallthrough]]` attribute modifies a `null statement` to indicate that fallthrough is intentional (and no warnings should be triggered)
                
                ```cpp
                    switch (2)
                    {
                    case 1:
                        std::cout << 1 << '\n';
                        break;
                    case 2:
                        std::cout << 2 << '\n'; // Execution begins here
                        [[fallthrough]]; // intentional fallthrough -- note the semicolon to indicate the null statement
                    case 3:
                        std::cout << 3 << '\n'; // This is also executed
                        break;
                    }
                ```
                
        - You can declare or define (but not initialize) variables inside the switch, both before and after the case labels
            - Initialization of variables is disallowed in any case that is not the last case (because the switch could jump over the initializer if there is a subsequent case defined, in which case the variable would be undefined, and accessing it would result in undefined behavior).
    - **[[Goto Statements|Goto statements]]**
        - Unconditional jumps are implemented via a **goto statement**, and the spot to jump to is identified through use of a **statement label**.
            
            ```cpp
            int main()
            {
                double x{};
            tryAgain: // this is a statement label
                std::cout << "Enter a non-negative number: ";
                std::cin >> x;
            
                if (x < 0.0)
                    goto tryAgain; // this is the goto statement
            
                std::cout << "The square root of " << x << " is " << std::sqrt(x) << '\n';
                return 0;
            }
            ```
            
        - Statement labels utilize a third kind of scope: **function scope**, which means the label is visible throughout the function even before its point of declaration.
            - The goto statement and its corresponding `statement label` must appear in the same function.
            - goto statements can also jump forward
                - You can jump only within the bounds of a single function (you can't jump out of one function and into another)
                - If you jump forward, you can't jump forward over the initialization of any variable that is still in scope at the location being jumped to.
                    - Note that you can jump backwards over a variable initialization, and the variable will be re-initialized when the initialization is executed.
- **[[Loops]]**
    - Integral loop variables should generally be a signed integral type.
    - **[[Do-While Loop|Do while statements]]**
        
        ```
        do
            statement; // can be a single statement or a compound statement
        while (condition);
        ```
        
        - A **do while statement** is a looping construct that works just like a while loop, except the statement always executes at least once.
        - Favor while loops over do-while when given an equal choice.
    - **[[For Loop|For Loops]]**
        - **Multiple counters**
        
        ```cpp
            for (int x{ 0 }, y{ 9 }; x < 10; ++x, --y)
                std::cout << x << ' ' << y << '\n';
        ```
        
- **[[Halts|Halts (exiting your program early)]]**
    - A **halt** is a flow control statement that terminates the program.
    - **std::exit()**
        - `std::exit()` is a function that causes the program to terminate normally.
            - return value from `main()` (the `status code`) passed in as an argument.
            - **Normal termination** means the program has exited in an expected way. Note that the term `normal termination` does not imply anything about whether the program was successful (that's what the `status code` is for).
            - Objects with static storage duration are destroyed. Then, some other miscellaneous file cleanup is done if any files were used. Finally, control is returned back to the OS, with the argument passed to `std::exit()` used as the `status code`.
                - **Does not clean up local variables**
            - Called implicitly after function `main()` returns
                - Can also be called explicitly to halt the program before it would normally terminate.
        - **std::atexit**
            
            ```cpp
            void cleanup()
            {
                // code here to do any kind of cleanup required
                std::cout << "cleanup!\n";
            }
            
            int main()
            {
             // register cleanup() to be called automatically when std::exit() is called
                std::atexit(cleanup); // note: we use cleanup rather than cleanup() since we're not making a function call to cleanup() right now
            
                std::cout << 1 << '\n';
            
                std::exit(0); // terminate and return status code 0 to operating system
            
                // The following statements never execute
                std::cout << 2 << '\n';
            
                return 0;
            }
            ```
            
            - The function being registered must take no parameters and have no return value.
            - Can register multiple cleanup functions using `std::atexit()` if you want, and they will be called in reverse order of registration (the last one registered will be called first).
            - In multi-threaded programs, calling `std::exit()` can cause your program to crash (because the thread calling `std::exit()` will cleanup static objects that may still be accessed by other threads).
                - For this reason, C++ has introduced another pair of functions that work similarly to `std::exit()` and `std::atexit()` called `std::quick_exit()` and `std::at_quick_exit()`.
                    - `std::quick_exit()` terminates the program normally, but does not clean up static objects, and may or may not do other types of cleanup.
                    - `std::at_quick_exit()` performs the same role as `std::atexit()` for programs terminated with `std::quick_exit()`.
    - **std::abort and std::terminate**
        - The `std::abort()` function causes your program to terminate abnormally.
            - **Abnormal termination** means the program had some kind of unusual runtime error and the program couldn't continue to run.
            - `std::abort()` does not do any cleanup.
            - The `std::terminate()` function is typically used in conjunction with `exceptions`
                - By default, `std::terminate()` calls `std::abort()`.
- **[[Random Number Generation|Random number generation]]**
    - **Algorithms and state**
        - An **algorithm** is a finite sequence of instructions that can be followed to solve some problem or produce some useful result.
        - An algorithm is considered to be **stateful** if it retains some information across calls.
        - Conversely, a **stateless** algorithm does not store any information (and must be given all the information it needs to work with whenever it is called).
        - The term **state** refers to the current values held in stateful variables (those retained across calls).
    - **[[Pseudo-random Number Generators|Pseudo-random number generators (PRNGs)]]**
        - An algorithm that generates a sequence of numbers whose properties simulate a sequence of random numbers.
        - **Seeding a PRNG**
            - The value (or set of values) used to set the initial state of a PRNG is called a **random seed** (or **seed** for short).
                - When the initial state of the PRNG has been set using a seed value(s), we say it has been **seeded.**
                - Because the initial state of the PRNG is set from the seed value(s), all of the values that a PRNG will produce are deterministically calculated from the seed value(s).
                - If a PRNG is not provided with enough bits of quality seed data, we say that it is **underseeded**. An underseeded PRNG may begin to produce randomized results whose quality is compromised in some way -- and the more severe the underseeding, the more the quality of the results will suffer.
                - *An ideal seed should have the following characteristics:*
                    - The seed should contain at least as many bits as the state of the PRNG, so that every bit in the state of the PRNG can be initialized by an independent bit in the seed.
                    - Each bit in the seed should be independently randomized.
                    - The seed should contain a good mix of 0 and 1s distributed across all of the bits.
                    - There should be no bits in the seed that are always 0 or always 1. These "stuck bits" do not provide any value.
                    - The seed should have a low correlation with previously generated seeds.
    - **[[Mersenne Twister]]**
        - `#include <random> // for std::mt19937`
            - `mt19937` is a Mersenne Twister that generates 32-bit unsigned integers
            - `mt19937_64` is a Mersenne Twister that generates 64-bit unsigned integers
        - `std::uniform_int_distribution`
        - **Seeding with the system clock**
            - Use `std::random_device{}()` to seed your PRNGs (unless it's not implemented properly for your target compiler/architecture).
                - Creates a value-initialized temporary object of type `std::random_device`.
            - Only seed a given pseudo-random number generator once, and do not reseed it.
