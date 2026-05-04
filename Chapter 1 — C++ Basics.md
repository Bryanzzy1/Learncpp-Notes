# Chapter 1 — C++ Basics

Pin: No
Fav: No
Category: Done (https://www.notion.so/Done-24fb13b569b5836bb160816e5fa04870?pvs=21)
Created: April 13, 2026 8:45 PM
Last edited: April 14, 2026 7:03 PM

## Notes

- **Initialization**
    - Default-initialization and copy-initialization have fallen out of favor in modern C++ due to inefficient initialization for complex types compared to other initializations
        - Direct initialization is also used when values are explicitly cast to another type (e.g., via `static_cast`).
            
            ```cpp
            int a;         // default-initialization (no initializer)
            // Traditional initialization forms:
            int b = 5;     // copy-initialization (initial value after equals sign)
            int c ( 6 );   // direct-initialization (initial value in parenthesis)
            ```
            
    - **List-initialization** (or **uniform initialization** or **brace initialization**) is the modern way of initializing C++ objects ***(PREFERRED)***
        
        ```cpp
        int d { 7 };   // direct-list-initialization (initial value in braces)
        int e {};      // value-initialization/zero-initialization to value 0
        ```
        
        - Disallow Narrowing conversions:
            
            ```cpp
                int w1 { 4.5 }; // compile error: list-init does not allow narrowing conversion
            
                int w2 = 4.5;   // compiles: w2 copy-initialized to value 4
                int w3 (4.5);   // compiles: w3 direct-initialized to value 4
            ```
            
    - For class types, value-initialization (and default-initialization) may instead initialize the object to predefined default values, which may be non-zero.
    - Other types of initialization (Later)
        - Aggregate initialization (see 13.8 -- Struct aggregate initialization).
        - Reference initialization (see 12.3 -- Lvalue references).
        - Static-initialization, constant-initialization, and dynamic-initialization (see 7.8 -- Why (non-const) global variables are evil).
        
- **Instantiation**
    - **Instantiation** is a fancy word that means a variable has been created (allocated) and initialized (this includes default initialization)
        - The `[[maybe_unused]]` The attribute should only be applied selectively to variables that have a specific and legitimate reason for being unused
    
    ```cpp
    [[maybe_unused]] double pi { 3.14159 };  // Don't complain if pi is unused
    ```
    
- **I/O**
    - Output a newline **`std::endl`** whenever a line of output is complete.
    - **`std::cout` and `std::cin`**is buffered
        - Periodically, the buffer is **flushed,** meaning all of the data collected in the buffer is transferred to its destination (in this case, the console).
        - If your program crashes, aborts, or is paused before the buffer is flushed, any output still waiting in the buffer will not be displayed.
        - use **`\n`** to avoid flushing the buffer **(PREFERRED)**
        - Leading whitespace characters (spaces, tabs, and newlines at the front of the buffer) are discarded from the input buffer.
        - If the extraction fails after `>>` extracted, the object being extracted to is copy-assigned the value `0` (as of C++11), and any future extractions will immediately fail (until `std::cin` is cleared).
- **Initializaition**
    - Initialized = The object is given a known value at the point of definition.
    - Assignment = The object is given a known value beyond the point of definition.
    - Uninitialized = The object has not been given a known value yet.
    - Using an uninitialized variable will cause the computer to assign it some unused memory (Unknown/unexpected results)
- Avoid implementation-defined and unspecified behavior whenever possible, as they may cause your program to malfunction on other implementations.
- In C++, the term “*side effect*” has a different meaning: it is an observable effect of an operator or function beyond producing a return value.
    - Both `operator=` and `operator<<` (when used to output values to the console) return their left operand. Thus, `x = 5` returns `x`, and `std::cout << 5` returns `std::cout`. This is done so that these operators can be chained.
        - For example, `x = y = 5` evaluates as `x = (y = 5)`. First `y = 5` assigns `5` to `y`. This operation then returns `y`, which can then be assigned to `x`.