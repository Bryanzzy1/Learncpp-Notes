# Chapter 5 — Constants and Strings

Pin: No
Fav: No
Category: Done (https://www.notion.so/Done-24fb13b569b5836bb160816e5fa04870?pvs=21)
Created: April 13, 2026 8:45 PM
Last edited: April 24, 2026 6:43 PM

## Notes

- **Constants**
    - A value that may not be changed during the program’s execution.
    - **Named constants** are constant values that are associated with an identifier. These are also sometimes called **symbolic constants**.
    - **Literal constants** are constant values that are not associated with an identifier.
    - There are three ways to define a named constant in C++:
        - Constant variables
            - Const variables *must* be initialized when you define them
            - The initializer of a const variable can be a non-constant value.
            - Function parameters can be made constants
                - `void printInt(const int x)`
                - Don’t use `const` for value parameters.
            - **Const return values**
                - `const int getValue()`
                - Don’t use `const` when returning by value.
        - Object-like macros with substitution text
            - `#define MY_NAME "Alex"`
                - When the preprocessor processes the file containing this code, it will replace `MY_NAME` (on line 7) with `"Alex"`.
            - **Prefer constant variables to preprocessor macros**
        - Enumerated constants
    - ***Type Qualifiers***
        - A **type qualifier** (sometimes called a **qualifier** for short) is a keyword that is applied to a type that modifies how that type behaves.
            - The `const` used to declare a constant variable is declared using a **const type qualifier** (or **const qualifier** for short).
        - As of C++23, C++ only has two type qualifiers: `const` and `volatile`.
            - The `volatile` A qualifier is used to tell the compiler that an object's value may change at any time.
        - In technical documentation, the `const` and `volatile` qualifiers are often called **cv-qualifiers**.
            - In technical documentation, the const and volatile qualifiers are often referred to as CV-qualifiers.
                - A **cv-unqualified** type is a type with no type qualifiers (e.g. `int`).
                - A **cv-qualified** type is a type with one or more type qualifiers applied (e.g. `const int`).
                - A **possibly cv-qualified** type is a type that may be cv-unqualified or cv-qualified.
                - NOT SOMETHING TO REMEMBER
- **Literals**
    - All literals have a type.
        - The type of a literal is deduced from the literal’s value.
    - ***Literal suffix***
        - If the default type of a literal is not as desired, you can change the type of a literal by adding a suffix.
            
            
            | **Data type** | **Suffix** | **Meaning** |
            | --- | --- | --- |
            | integral | u or U | unsigned int |
            | integral | l or L | long |
            | integral | ul, uL, Ul, UL, lu, lU, Lu, LU | unsigned long |
            | integral | ll or LL | long long |
            | integral | ull, uLL, Ull, ULL, llu, llU, LLu, LLU | unsigned long long |
            | integral | z or Z | The signed version of std::size_t (C++23) |
            | integral | uz, uZ, Uz, UZ, zu, zU, Zu, ZU | std::size_t (C++23) |
            | floating point | f or F | float |
            | floating point | l or L | long double |
            | string | s | std::string |
            | string | sv | std::string_view |
        - Prefer literal suffix L (upper case) over l (lower case).
        - By default, floating point literals have a type of `double`. To make them `float` literals instead, the `f` (or `F`) suffix should be used:
        
        ```cpp
        #include <iostream>
        
        int main()
        {
            std::cout << 5.0 << '\n';  // 5.0 (no suffix) is type double (by default)
            std::cout << 5.0f << '\n'; // 5.0f is type float
        
            return 0;
        }
        
        float f { 4.1f }; // use 'f' suffix so the literal is a float and matches variable type of float
        double d { 4.1 }; // change variable to type double so it matches the literal type double
        ```
        
    - ***Binary Literals***
        - In C++14 onward, we can use binary literals by using the 0b prefix:
        `bin = 0b1;       // assign binary 0000 0000 0000 0001 to the variable`
        - C++14 also adds the ability to use a quotation mark (‘) as a digit separator.
            - `int bin { 0b1011'0010 };  // assign binary 1011 0010 to the variable`
            - `long value { 2'132'673'462 }; // much easier to read than 2132673462`
        - By default, C++ outputs values in decimal. However, you can change the output format via use of the `std::dec`, `std::oct`, and `std::hex` I/O manipulators:
            - `std::cout << std::hex << x << '\n'; // hexadecimal`
        - **Outputting values in binary**
            - `std::bitset`
                - Define a `std::bitset` variable and tell `std::bitset` how many bits we want to store.
                    
                    ```cpp
                    // std::bitset<8> means we want to store 8 bits
                    
                    // binary literal for binary 1100 0101
                    std::bitset<8> bin1{ 0b1100'0101 }; 
                    
                    // hexadecimal literal for binary 1100 0101
                    std::bitset<8> bin2{ 0xC5 }; 
                    
                    	std::cout << bin1 << '\n' << bin2 << '\n';
                    	std::cout << std::bitset<4>{ 0b1010 } << '\n'; // create a temporary std::bitset and print it
                    ```
                    
            - In C++20 and C++23, we have better options for printing binary via the new Format Library (C++20) and Print Library (C++23):
            
            ```cpp
            #include <format> // C++20
            #include <iostream>
            #include <print> // C++23
            
            int main()
            {
            		// C++20, {:b} formats the argument as binary digits
                std::cout << std::format("{:b}\n", 0b1010);  
                
                // C++20, {:#b} formats the argument as 0b-prefixed binary digits
                std::cout << std::format("{:#b}\n", 0b1010); 
            		
            		// C++23, format/print two arguments (same as above) and a newline
                std::println("{:b} {:#b}", 0b1010, 0b1010);  
                return 0;
            }
            ```
            
- **The as-if rule and compile-time optimization**
    - Modern C++ compilers are optimizing compilers, meaning they are capable of automatically optimizing your programs as part of the compilation process.
        - The **as-if rule** says that the compiler can modify a program however it likes in order to produce more optimized code, so long as those modifications do not affect a program’s “observable behavior”.
    - C**ompile-time evaluation**
        - When the compiler fully or partially evaluates an expression at compile-time
        - Compile-time evaluation allows the compiler to do work at compile-time that would otherwise be done at runtime.
            - **Constant folding**
                - The compiler replaces expressions that have literal operands with the result of the expression.
            - **Constant propagation**
                - The compiler replaces variables known to have constant values with their values
                - Const variables are easier to optimize
            - **Dead code elimination**
                - The compiler removes code that may be executed but has no effect on the program’s behavior.
                - When a variable is removed from a program because it is no longer needed, we say the variable has been **optimized out** (or **optimized away**)
        - Debug builds will typically leave optimizations turned off
    - **Compile-time constants vs runtime constants**
        - A **compile-time constant** is a constant whose value is known at compile-time.
            - Literals.
            - Constant objects whose initializers are compile-time constants.
        - A **runtime constant** is a constant whose value is determined in a runtime context.
            - Constant function parameters.
            - Constant objects whose initializers are non-constants or runtime constants.
    
    ```cpp
        // The following are non-constants:
        [[maybe_unused]] int a { 5 };
    
        // The following are compile-time constants:
        [[maybe_unused]] const int b { 5 };
        [[maybe_unused]] const double c { 1.2 };
        [[maybe_unused]] const int d { b };       // b is a compile-time constant
    
        // The following are runtime constants:
        [[maybe_unused]] const int e { a };       // a is non-const
        [[maybe_unused]] const int f { e };       // e is a runtime constant
        [[maybe_unused]] const int g { five() };  // return value isn't known until runtime
        [[maybe_unused]] const int h { pass(5) }; // return value isn't known until runtime
    ```
    
- **Constant expressions**
    - A **constant expression** is a non-empty sequence of literals, constant variables, operators, and function calls, all of which must be evaluatable at compile-time.
        - The initializer of a constexpr variable
            - `constexpr`
                - A **constexpr** variable is always a compile-time constant.
                    - Function parameters cannot be declared as `constexpr`, since their initialization value isn’t determined until runtime.
                - Require an initializer that can be evaluated at compile-time:
                - `constexpr int x { expr }; // Because variable x is constexpr, expr must be evaluatable at compile-time`
                - `constexpr` works for variables with non-integral types
                - **Constexpr Functions**
                    - Can be called in a constant expression
                    - All arguments must be constant expressions
        - The initializer of a constexpr variable
        - The initializer of a constexpr variable
    - The use of language features that result in compile-time evaluation is called **compile-time programming**.
        - Compile-time evaluation allows us to write programs that are both more performant and of higher quality
    - An expression that is not a constant expression is often called a non-constant expression, and may informally be called a **runtime expression**
- **std::string**
    - `#include <string>`
        - `std::string name { "Alex" }; // initialize name with string literal "Alex"`
    - `std::string` can store strings of different lengths
    - `operator>>` only returns characters up to the first whitespace it encounters.
        - **Use `std::getline()` to input text**
            - `std::getline(std::cin, name);`
    - `std::ws`
        - tells `std::cin` to ignore any leading whitespace before extraction.
        - `std::getline(std::cin >> std::ws, name);`
    - ***name.length()***
        - *member function*
            - With member functions, we call `object.function()`.
        - `std::string::length()` returns an unsigned integral value (most likely of type `size_t`)
            - `int length { static_cast<int>(name.length()) };`
            - In C++20, you can also use the `std::ssize()` function to get the length of a `std::string` as a large signed integral type (usually `std::ptrdiff_t`)
    - We can create string literals with type `std::string` by using a `s` suffix after the double-quoted string literal. The `s` must be lower case.
        - `"Hello"s` resolves to `std::string { "Hello", 5 }` which creates a temporary `std::string` initialized with C-style string literal “Hello” (which has a length of 5, excluding the implicit null-terminator).
    - ***std::string_view***
        - Introduced in C++17
        - Provides read-only access to an *existing* string (a C-style string, a `std::string`, or another `std::string_view`) without making a copy.
        - Prefer `std::string_view` over `std::string` when you need a read-only string, especially for function parameters.
        - **`std::string_view` can be initialized with many different types of strings**
        - C++ won’t allow the implicit conversion of a `std::string_view` to a `std::string`.
        - We can create string literals with type `std::string_view` by using a `sv` suffix after the double-quoted string literal.
        - Full support for constexpr
        - ***Owner VS Viewer***
            - **`std::string` is a (sole) owner**
            - **`std::string_view` is a viewer**
                - A view is dependent on the object being viewed. If the object being viewed is modified or destroyed while the view is still being used, unexpected or undefined behavior will result.
                - A `std::string_view` that is viewing a string that has been destroyed is sometimes called a **dangling** view.
                • If the `std::string` reallocates memory in order to accommodate the new string data, it will return the memory used for the old string data back to the operating system. Since the `std::string_view` is still viewing the old string data, it is now dangling (pointing to a now-invalid object).
                - f the `std::string` does not reallocate memory, it will copy the new string data over the old string data (starting at the same memory address). The `std::string_view` will now be viewing the new string data (since it was placed at the same memory address as it was viewing), but it will not realize that the length of the `std::string` has probably changed.
            - **View modification functions**
                - The `remove_prefix()` member function removes characters from the left side of the view.
                - The `remove_suffix()` member function removes characters from the right side of the view.