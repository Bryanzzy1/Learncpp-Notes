---
tags:
  - cpp/types
  - cpp/memory
  - cpp/integers
  - cpp/floats
  - cpp/casting
  - concept
  - syntax
  - chapter
aliases:
  - Fundamental Data Types
  - Data Types
  - Ch4
up: LearnCPP
related:
  - "[[Memory Model]]"
  - "[[Fundamental Data Types]]"
  - "[[Integers]]"
  - "[[Floating Point]]"
  - "[[Boolean]]"
  - "[[Char]]"
  - "[[size_t]]"
  - "[[static_cast]]"
---

# Chapter 4 — Fundamental Data Types

Pin: No
Fav: No
Category: Done (https://www.notion.so/Done-24fb13b569b5836bb160816e5fa04870?pvs=21)
Created: April 13, 2026 8:45 PM
Last edited: April 22, 2026 1:17 PM

## Notes

- **[[Memory Model|Memory]]**
    - Memory is organized into sequential units called **memory addresses**
    - The following picture shows some sequential memory addresses, along with the corresponding byte of data:
        
    - When you give an object a value:
        - The compiler and CPU take care of encoding your value into the appropriate sequence of bits for that data type
        - Then stored in memory (remember: memory can only store bits).
    - Every time we define an object, a small portion of that free memory is used for as long as the object is in existence.
- **[[Fundamental Data Types|Fundamental data types]]**
    
    
    | **Types** | **Category** | **Meaning** | **Example** |
    | --- | --- | --- | --- |
    | floatdoublelong double | Floating Point | a number with a fractional part | 3.14159 |
    | bool | Integral (Boolean) | true or false | true |
    | charwchar_tchar8_t (C++20)char16_t (C++11)char32_t (C++11) | Integral (Character) | a single character of text | ‘c’ |
    | short intintlong intlong long int (C++11) | Integral (Integer) | positive and negative whole numbers, including 0 | 64 |
    | std::nullptr_t (C++11) | Null Pointer | a null pointer | nullptr |
    | void | Void | no type | n/a |
    - The integral types are bool, the various char types, and the standard integer types.
    - An **incomplete type** is a type that has been declared but not yet defined.
        - Void is our first example of an incomplete type.
    - The C++ standard does not define the exact size (in bits) of any of the fundamental types.
        - An object must occupy at least 1 byte (so that each object has a distinct memory address).
        - A byte must be at least 8 bits.
        - The integral types `char`, `short`, `int`, `long`, and `long long` have a minimum size of 8, 16, 16, 32, and 64 bits respectively.
        - `char` and `char8_t` are exactly 1 byte (at least 8 bits).
        - When we talk about the size of a type, we really mean the size of an [[Initialization|instantiated]] object of that type.
        
        | **Category** | **Type** | **Minimum Size** | **Typical Size** |
        | --- | --- | --- | --- |
        | Boolean | bool | 1 byte | 1 byte |
        | Character | char | 1 byte (exactly) | 1 byte |
        |  | wchar_t | 1 byte | 2 or 4 bytes |
        |  | char8_t | 1 byte | 1 byte |
        |  | char16_t | 2 bytes | 2 bytes |
        |  | char32_t | 4 bytes | 4 bytes |
        | Integral | short | 2 bytes | 2 bytes |
        |  | int | 2 bytes | 4 bytes |
        |  | long | 4 bytes | 4 or 8 bytes |
        |  | long long | 8 bytes | 8 bytes |
        | Floating point | float | 4 bytes | 4 bytes |
        |  | double | 8 bytes | 8 bytes |
        |  | long double | 8 bytes | 8, 12, or 16 bytes |
        | Pointer | std::nullptr_t | 4 bytes | 4 or 8 bytes |
    - The **sizeof operator** is a unary operator that takes either a type or a variable, and returns the size of an object of that type (in bytes).
        - Trying to use `sizeof` on an incomplete type (such as `void`) will result in a compilation error.
        - `sizeof` does not include dynamically allocated memory used by an object.
        - `sizeof` returns a value of type `std::size_t`
            - The compiler decides if `std::size_t` is an unsigned int, an unsigned long, an unsigned long long, etc…
            - `std::size_t` is actually a typedef.
            - The byte size of an object can be no larger than the largest value `std::size_t` can hold.
            - Creating an object with a size (in bytes) larger than the largest value an object of type `std::size_t` can hold is invalid (and will cause a compile error).
    - ***[[Integers|Signed Ints]]***
        - By default, integers in C++ are **signed**
        - An n-bit signed variable has a range of -(2^(n-1)) to (2^(n-1))-1.
        - Attempts to create a value outside the range that can be represented this is called **integer overflow** (or **arithmetic overflow**).
    - ***[[Integers|Unsigned Ints]]***
        - If an unsigned value is out of range, it is divided by one greater than the largest number of the type, and only the remainder is kept.
        - Favor signed numbers over unsigned numbers for holding quantities (even quantities that should be non-negative) and mathematical operations. Avoid mixing signed and unsigned numbers.
    - ***[[Integers|Overflow]]***
        - “If during the evaluation of an expression, the result is not mathematically defined or not in the range of representable values for its type, the behavior is undefined”.
    - ***[[Integers|Fixed-width integers]]***
        
        
        | **Name** | **Fixed Size** | **Fixed Range** | **Notes** |
        | --- | --- | --- | --- |
        | std::int8_t | 1 byte signed | -128 to 127 | Treated like a signed char on many systems. See note below. |
        | std::uint8_t | 1 byte unsigned | 0 to 255 | Treated like an unsigned char on many systems. See note below. |
        | std::int16_t | 2 byte signed | -32,768 to 32,767 |  |
        | std::uint16_t | 2 byte unsigned | 0 to 65,535 |  |
        | std::int32_t | 4 byte signed | -2,147,483,648 to 2,147,483,647 |  |
        | std::uint32_t | 4 byte unsigned | 0 to 4,294,967,295 |  |
        | std::int64_t | 8 byte signed | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |  |
        | std::uint64_t | 8 byte unsigned | 0 to 18,446,744,073,709,551,615 |  |
        - Use a fixed-width integer type when you need an integral type that has a guaranteed range.
        - Modern compilers typically treat `std::int8_t` and `std::uint8_t` (and the corresponding fast and least fixed-width types) the same as `signed char` and `unsigned char` respectively.
    - ***[[Integers|Fast and least integral types]]***
        - `std::int_fast32_t` will give you the fastest signed integer type that’s at least 32 bits. By fastest, we mean the integral type that can be processed most quickly by the CPU.
        - `std::uint_least32_t` will give you the smallest unsigned integer type that’s at least 32 bits.
        - Avoid the fast, least-integral types because they may exhibit different behavior on architectures where they resolve to different sizes.
    - Here for best practices with ints: https://www.learncpp.com/cpp-tutorial/fixed-width-integers-and-size-t/
    - ***[[Floating Point|Floats]]***
        
        
        | **C++ Type** | **Typical Size** |
        | --- | --- |
        | float | 4 bytes |
        | double | 8 bytes |
        | long double | 8, 12, or 16 bytes |
        - Favor double over float unless space is at a premium, as the lack of precision in a float will often lead to inaccuracies.
        - Always make sure the type of your literals matches the type of the variables they’re being assigned to or used to initialize. Otherwise, an unnecessary conversion will result, possibly with a loss of precision.
        - The **precision** of a floating-point type defines how many significant digits it can represent without information loss.
        - We can override the default precision that std::cout shows by using an `output manipulator` function named `std::setprecision()`. **Output manipulators** alter how data is output, and are defined in the *iomanip* header.
        - IEEE 754 compatible formats additionally support some special values:
            - **Inf**, which represents infinity. Inf is signed, and can be positive (+Inf) or negative (-Inf).
            - **NaN**, which stands for “Not a Number”. There are several different kinds of NaN (which we won’t discuss here).
            - Signed zero, meaning there are separate representations for “positive zero” (+0.0) and “negative zero” (-0.0).
    - **[[Boolean]]**
        - `bool b3 {}; // default initialize to false`
        - They evaluate to the integers `0` (false) or `1` (true).
            - When we print Boolean values, [[IO|std::cout]] prints `0` for `false`, and `1` for `true`
            - Use `std::boolalpha` to print `true` or `false`
            - By default, `std::cin` only accepts numeric input for Boolean variables: `0` is `false`, and `1` is `true`.
    - **[[Char]]**
        - When using std::cout to print a char, std::cout outputs the char variable as an ASCII character
        - Char is defined by C++ to always be 1 byte in size.
        - A signed char can hold a number between -128 and 127. An unsigned char can hold a number between 0 and 255.
            
            
            | **Name** | **Symbol** | **Meaning** |
            | --- | --- | --- |
            | Alert | \a | Makes an alert, such as a beep |
            | Backspace | \b | Moves the cursor back one space |
            | Formfeed | \f | Moves the cursor to next logical page |
            | Newline | \n | Moves cursor to next line |
            | Carriage return | \r | Moves cursor to beginning of line |
            | Horizontal tab | \t | Prints a horizontal tab |
            | Vertical tab | \v | Prints a vertical tab |
            | Single quote | \’ | Prints a single quote |
            | Double quote | \” | Prints a double quote |
            | Backslash | \\ | Prints a backslash. |
            | Question mark | \? | Prints a question mark.No longer relevant. You can use question marks unescaped. |
            | Octal number | \(number) | Translates into char represented by octal |
            | Hex number | \x(number) | Translates into char represented by hex number |
        - Text between double quotes (e.g. “Hello, world!”) is treated as a C-style string literal, which can contain multiple characters.
            - Avoid multicharacter literals (e.g. `'56'`).
- **[[static_cast|Type Casting]]**
    - When the compiler does type conversion on our behalf without us explicitly asking, we call this **implicit type conversion**.
        - Some advanced type conversions (e.g. those involving `const_cast` or `reinterpret_cast`) do not return temporary objects, but instead reinterpret the type of an existing value or object.
    - To perform an explicit type conversion, in most cases we’ll use the `static_cast` operator.
        - `static_cast<new_type>(expression)`
        - `print( static_cast<int>(5.5) ); // explicitly convert double value 5.5 to an int`
        - When we pass in a variable, that variable is evaluated to produce its value, and that value is then converted to the new type.
    - If the value being converted cannot be represented in the destination type:
        - If the destination type is unsigned, the value will be modulo wrapped.
        - If the destination type is signed, the value is implementation-defined before C++20, and will be modulo wrapped as of C++20.
    - ***std::int8_t and std::uint8_t likely behave like chars instead of integers***
        - Most compilers define and treat `std::int8_t` and `std::uint8_t` (and the corresponding fast and least fixed-width types) identically to types `signed char` and `unsigned char` respectively.
