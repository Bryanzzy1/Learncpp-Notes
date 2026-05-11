---
tags:
  - cpp/types
  - cpp/casting
  - cpp/type-deduction
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Type Conversion
  - Ch10
up: LearnCPP
related:
  - "[[Implicit Type Conversion]]"
  - "[[Standard Conversions]]"
  - "[[Numeric Promotion]]"
  - "[[Numeric Conversions]]"
  - "[[Narrowing Conversions]]"
  - "[[Arithmetic Conversions]]"
  - "[[Explicit Type Conversion]]"
  - "[[Type Aliases]]"
  - "[[Type Deduction]]"
  - "[[Type Deduction for Functions]]"
---

# Chapter 10 — Type conversion

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 11, 2026 4:16 PM

## Notes

- **[[Implicit Type Conversion]]**
    - Performed automatically by the compiler when an expression of some type is supplied in a context where some other type is expected.
    - **[[Standard Conversions]]**
        - Specify how various fundamental types (and certain compound types, including arrays, references, pointers, and enumerations) convert to other types within that same group.
        
        | **Category** | **Meaning** | **Link** |
        | --- | --- | --- |
        | Numeric promotions | Conversions of small integral types to `int` or `unsigned int`, and of `float` to `double`. | [10.2 -- Floating-point and integral promotion](https://www.learncpp.com/cpp-tutorial/floating-point-and-integral-promotion/) |
        | Numeric conversions | Other integral and floating point conversions that aren't promotions. | [10.3 -- Numeric conversions](https://www.learncpp.com/cpp-tutorial/numeric-conversions/) |
        | Qualification conversions | Conversions that add or remove `const` or `volatile`. |  |
        | Value transformations | Conversions that change the value category of an expression | [12.2 -- Value categories (lvalues and rvalues)](https://www.learncpp.com/cpp-tutorial/value-categories-lvalues-and-rvalues/) |
        | Pointer conversions | Conversions from `std::nullptr` to pointer types, or pointer types to other pointer types |  |
    - **Type conversion can fail**
        - If the compiler can't find an acceptable conversion, then the compilation will fail with a compile error.
- **[[Numeric Promotion]]**
    - **Numeric promotion**
        - The type conversion of certain narrower numeric types (such as a `char`) to certain wider numeric types (typically `int` or `double`) that can be processed efficiently.
        - value-preserving conversion
        - Integral promotions
            - signed char or signed short can be converted to int.
            - unsigned char, char8_t, and unsigned short can be converted to int if int can hold the entire range of the type, or unsigned int otherwise.
            - If char is signed by default, it follows the signed char conversion rules above. If it is unsigned by default, it follows the unsigned char conversion rules above.
            - bool can be converted to int, with false becoming 0 and true becoming 1.
        - Floating point promotions
            - A value of type `float` can be converted to a value of type `double`.
        - Not all widening conversions are numeric promotions
    - A **value-preserving conversion** (also called a **safe conversion**) is one where every possible source value can be converted into an equal value of the destination type.
- **[[Numeric Conversions]]**
    - 5 basic types of numeric conversions
        1. Converting an integral type to any other integral type (excluding integral promotions)
        2. Converting a floating point type to any other floating point type (excluding floating point promotions)
        3. Converting a floating point type to any integral type
        4. Converting an integral type to any floating point type
        5. Converting an integral type or a floating point type to a bool
    - An **unsafe conversion** is one where at least one value of the source type cannot be converted into an equal value of the destination type.
        - Numeric conversions fall into three general safety categories:
            1. *Value-preserving conversions* are safe numeric conversions where the destination type can exactly represent all possible values in the source type.
                1. For example, `int` to `long` and `short` to `double` are safe conversions, as the source value can always be converted to an equal value of the destination type.
            2. *Reinterpretive conversions* are unsafe numeric conversions where the converted value may be different than the source value, but no data is lost. Signed/unsigned conversions fall into this category.
                1. C++20 now requires that signed integers use 2s complement.
            3. *Lossy conversions* are unsafe numeric conversions where data may be lost during the conversion.
    - Numeric conversions are not numeric promotions as they are not carried out for performance purposes.
    - Numeric conversions are made to represent a value in the target type, and they may or may not be safe.
- **[[Narrowing Conversions]]**
    - The following conversions are defined to be narrowing:
        - From a floating point type to an integral type.
        - From a floating point type to a narrower or lesser ranked floating point type, unless the value being converted is constexpr and is in range of the destination type (even if the destination type doesn't have the precision to store all the significant digits of the number).
        - From an integral to a floating point type, unless the value being converted is constexpr and whose value can be stored exactly in the destination type.
        - From an integral type to another integral type that cannot represent all values of the original type, unless the value being converted is constexpr and whose value can be stored exactly in the destination type. This covers both wider to narrower integral conversions, as well as integral sign conversions (signed to unsigned, or vice-versa).
    - If you need to perform a narrowing conversion, use `static_cast` to convert it into an explicit conversion.
    - **Brace initialization disallows narrowing conversions**
    - **List initialization with constexpr initializers**
        
        ```cpp
        int main()
        {
            // We can avoid literals with suffixes
            unsigned int u { 5 }; // okay (we don't need to use `5u`)
            float f { 1.5 };      // okay (we don't need to use `1.5f`)
        
            // We can avoid static_casts
            constexpr int n{ 5 };
            double d { n };       // okay (we don't need a static_cast here)
            short s { 5 };        // okay (there is no suffix for short, we don't need a static_cast here)
        
            return 0;
        }
        ```
        
- **[[Arithmetic Conversions]]**
    - The following rules are applied to find a matching type:
        - If one operand is an integral type and the other a floating point type, the integral operand is converted to the type of the floating point operand (no integral promotion takes place).
        - Otherwise, any integral operands are numerically promoted
        - After promotion, if one operand is signed and the other unsigned, special rules apply
            - If the rank of the unsigned operand is greater than or equal to the rank of the signed operand, the signed operand is converted to the type of the unsigned operand.
            - If the type of the signed operand can represent all the values of the type of the unsigned operand, the type of the unsigned operand is converted to the type of the signed operand.
            - Otherwise both operands are converted to the corresponding unsigned type of the signed operand.
        - Otherwise, the operand with lower rank is converted to the type of the operand with higher rank.
    - **`std::common_type` and `std::common_type_t`**
        - both defined in the <type_traits> header
        - `std::common_type_t<int, double>` returns the common type of `int` and `double`
        - `std::common_type_t<unsigned int, long>` returns the common type of `unsigned int` and `long`.
- **[[Explicit Type Conversion]]**
    
    | **Cast** | **Description** | **Safe?** |
    | --- | --- | --- |
    | static_cast | Performs compile-time type conversions between related types. | Yes |
    | dynamic_cast | Performs runtime type conversions on pointers or references in an polymorphic (inheritance) hierarchy | Yes |
    | const_cast | Adds or removes const. | Only for adding const |
    | reinterpret_cast | Reinterprets the bit-level representation of one type as if it were another type | No |
    | C-style casts | Performs some combination of `static_cast`, `const_cast`, or `reinterpret_cast`. | No |
    - `const_cast` and `reinterpret_cast` should generally be avoided because they are only useful in rare cases and can be harmful if used incorrectly.
    - **C-style casts**
        - `std::cout << (double)x / y << '\n'; // C-style cast of x to double`
        - **function-style cast**
            - `std::cout << double(x) / y << '\n'; //  // function-style cast of x to double`
        - Avoid using C-style casts.
    - **`static_cast` should be used to cast most values**
        - `std::cout << static_cast<int>(c) << '\n'; // prints 97 rather than a`
        - **Using `static_cast` to make narrowing conversions explicit**
    - Let's say we have some variable `x` that we need to convert to an `int`. There are two conventional ways we can do this:
        1. `static_cast<int>(x)`, which returns a temporary `int` object *direct-initialized* with `x`.
        2. `int { x }`, which creates a temporary `int` object *direct-list-initialized* with `x`.
            1. `int { x }` uses list initialization, which disallows narrowing conversions.
- **[[Type Aliases]]**
    - In C++, **using** is a keyword that creates an alias for an existing data type.
        - `using Distance = double; // define Distance as an alias for type double`
        - Once defined, a type alias can be used anywhere a type is needed.
            - `Distance milesToDestination{ 3.4 }; // defines a variable of type double`
        - Type aliases can also be templated.
        - Name your type aliases starting with a capital letter and do not use a suffix (unless you have a specific reason to do otherwise).
    - **Type aliases are not distinct types**
        - Aliases are not **type safe**.
        - Some languages support the concept of a **strong typedef** (or strong type alias).
        - A strong typedef actually creates a new type that has all the original properties of the original type, but the compiler will throw an error if you try to mix values of the aliased type and the strong typedef.
            - As of C++20, C++ does not directly support strong typedefs
    - **The scope of a type alias**
        - Type aliases follow the same scoping rules as variable identifiers: a type alias defined inside a block has block scope and is usable only within that block, whereas a type alias defined in the global namespace has global scope and is usable to the end of the file.
    - A **typedef** (which is short for "type definition") is an older way of creating an alias for a type.
        
        ```cpp
        // The following aliases are identical
        typedef long Miles;
        using Miles = long;
        ```
        
        - Prefer type aliases over typedefs.
    - Why use type aliases?
        - **Using type aliases for platform independent coding**
        - **Using type aliases to make complex types easier to read**
        - **Using type aliases to document the meaning of a value**
        - **Using type aliases for easier code maintenance**
- **[[Type Deduction]]**
    - **Type deduction** (also sometimes called **type inference**) is a feature that allows the compiler to deduce the type of an object from the object's initializer.
        
        ```cpp
        int main()
        {
            auto d { 5.0 }; // 5.0 is a double literal, so d will be deduced as a double
            auto i { 1 + 2 }; // 1 + 2 evaluates to an int, so i will be deduced as an int
            auto x { i }; // i is an int, so x will be deduced as an int
        
            return 0;
        }
        ```
        
    - Because function calls are valid expressions, we can even use type deduction when our initializer is a non-void function call
        - `auto sum { add(5, 6) }; // add() returns an int, so sum's type will be deduced as an int`
    - Type deduction will not work for objects that either do not have initializers or have empty initializers.
        - It also will not work when the initializer has type `void` (or any other incomplete type).
    - **Type deduction drops `const` from the deduced type**
        - Variables using type deduction may also use other specifiers/qualifiers, such as `const` or `constexpr`
    - **Type deduction for string literals**
        - `auto s { "Hello, world" }; // s will be type const char*, not std::string`
            - If you want the type deduced from a string literal to be `std::string` or `std::string_view`, you'll need to use the `s` or `sv` literal suffixes
    - Use type deduction for your variables when the type of the object doesn't matter.
    - Favor an explicit type when you require a specific type that differs from the type of the initializer, or when your object is used in a context where making the type obvious is useful.
- **[[Type Deduction for Functions]]**
    
    ```cpp
    auto add(int x, int y)
    {
        return x + y;
    }
    ```
    
    - When using an `auto` return type, all return statements within the function must return values of the same type, otherwise an error will result.
    - **Downsides**
        - Functions that use an `auto` return type must be fully defined before they can be used (a forward declaration is not sufficient).
        - Prefer explicit return types over return type deduction (except in cases where the return type is unimportant, difficult to express, or fragile).
    - The `auto` keyword can also be used to declare functions using a **trailing return syntax**, where the return type is specified after the rest of the function prototype.
    
    ```cpp
    auto add(int x, int y) -> int
    {
      return (x + y);
    }
    ```
    
    - **Type deduction can't be used for function parameter types**
        - But after C++20, it is triggering a different feature called `function templates` that was designed to actually handle such cases.
