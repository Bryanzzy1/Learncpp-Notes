---
tags:
  - cpp/functions
  - cpp/templates
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Function Overloading and Templates
  - Ch11
up: LearnCPP
related:
  - "[[Function Overloading]]"
  - "[[Overload Resolution]]"
  - "[[Deleted Functions]]"
  - "[[Default Arguments]]"
  - "[[Function Templates]]"
  - "[[Non-type Template Parameters]]"
---

# Chapter 11 — Functions

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 13, 2026 3:29 PM

## Notes

- **[[Function Overloading]]**
    - Create multiple functions with the same name, so long as each identically named function has different parameter types (or the functions can be otherwise differentiated).
    - Each function sharing a name (in the same scope) is called an **overloaded function** (sometimes called an **overload** for short).
    
    ```cpp
    int add(int x, int y) // integer version
    {
        return x + y;
    }
    
    double add(double x, double y) // floating point version
    {
        return x + y;
    }
    
    int main()
    {
        return 0;
    }
    ```
    
    - Functions can be overloaded so long as each overloaded function can be differentiated by the compiler. If an overloaded function can not be differentiated, a compile error will result.
    - **[[Overload Resolution]]**
        - When a function call is made to a function that has been overloaded, the compiler will try to match the function call to the appropriate overload based on the arguments used in the function call.
        - When a function call is made to an overloaded function, the compiler steps through a sequence of rules to determine which (if any) of the overloaded functions is the best match
        - At each step, the compiler applies a bunch of different type conversions to the argument(s) in the function call.
            - For each conversion applied, the compiler checks if any of the overloaded functions are now a match.
            - The **trivial conversions** are a set of specific conversion rules that will modify types (without modifying the value) for purposes of finding a match.
            - If no exact match is found, the compiler tries to find a match by applying numeric promotion to the argument(s).
            - If no match is found via numeric promotion, the compiler tries to find a match by applying numeric conversions
            - If no match is found via numeric conversion, the compiler tries to find a match through any user-defined conversions.
                
                ```cpp
                class X // this defines a new type called X
                {
                public:
                    operator int() { return 0; } // Here's a user-defined conversion from X to int
                };
                ```
                
                - The constructor of a class also acts as a user-defined conversion from other types to that class type, and can be used during this step to find matching functions.
            - If no match is found via user-defined conversion, the compiler will look for a matching function that uses ellipsis.
        - **Ambiguous matches**
            - An **ambiguous match** occurs when the compiler finds two or more functions that can be made to match in the same step.
                - When this occurs, the compiler will stop matching and issue a compile error stating that it has found an ambiguous function call.
            - Default arguments can also cause ambiguous matches.
        - If there are multiple arguments, the compiler applies the matching rules to each argument in turn.
            - The function chosen must provide a better match than all the other candidate functions for at least one parameter, and no worse for all of the other parameters.
    - In order for a program using overloaded functions to compile, two things have to be true:
        1. Each overloaded function has to be differentiated from the others.
        2. Each call to an overloaded function has to resolve to an overloaded function. 
        
        | **Function property** | **Used for differentiation** | **Notes** |
        | --- | --- | --- |
        | Number of parameters | Yes |  |
        | Type of parameters | Yes | Excludes typedefs, type aliases, and const qualifier on value parameters. Includes ellipses. |
        | Return type | No |  |
    - For member functions, additional function-level qualifiers are also considered:
        
        
        | **Function-level qualifier** | **Used for overloading** |
        | --- | --- |
        | const or volatile | Yes |
        | Ref-qualifiers | Yes |
    - **Overloading based on the type of parameters**
        - Because type aliases (or typedefs) are not distinct types, overloaded functions using type aliases are not distinct from overloads using the aliased type.
        - Ellipsis parameters are considered to be a unique type of parameter
    - **The return type of a function is not considered for differentiation**
    - A function's **type signature** (generally called a **signature**) is defined as the parts of the function header that are used for differentiation of the function.
        - In C++, this includes the function name, number of parameters, parameter type, and function-level qualifiers. It notably does *not* include the return type.
    - When the compiler compiles a function, it performs **name mangling**
        - The compiled name of the function is altered ("mangled") based on various criteria, such as the number and type of parameters, so that the linker has unique names to work with.
            - For example, a function with prototype `int fcn()` might compile to mangled name `__fcn_v`, whereas `int fcn(int)` might compile to mangled name `__fcn_i`.
- **[[Deleted Functions]]**
    
    ```cpp
    #include <iostream>
    
    void printInt(int x)
    {
        std::cout << x << '\n';
    }
    
    void printInt(char) = delete; // calls to this function will halt compilation
    void printInt(bool) = delete; // calls to this function will halt compilation
    
    int main()
    {
        printInt(97);   // okay
    
        printInt('a');  // compile error: function deleted
        printInt(true); // compile error: function deleted
    
        printInt(5.0);  // compile error: ambiguous match
    
        return 0;
    }
    ```
    
    - First, `printInt('a')` is a direct match for `printInt(char)`, which is deleted.
        - The compiler thus produces a compilation error.
    - `printInt(true)` is a direct match for `printInt(bool)`, which is deleted, and thus also produces a compilation error.
    - `= delete` means "I forbid this", not "this doesn't exist".
    - Deleted function participate in all stages of function overload resolution (not just in the exact match stage). If a deleted function is selected, then a compilation error results.
        - Other types of functions can be similarly deleted.
        - **Deleting all non-matching overloads**
            - We can do this by using a function template
                
                ```cpp
                //This function template will take precedence for arguments of other types
                //Since this function template is deleted, calls to it will halt compilation
                template <typename T>
                void printInt(T x) = delete;
                ```
                
- **[[Default Arguments]]**
    - A **default argument** is a default value provided for a function parameter.
        - Note that you must use the equals sign to specify a default argument. Using parenthesis or brace initialization won't work
    - Default arguments are inserted by the compiler at site of the function call.
    - If a parameter is given a default argument, all subsequent parameters (to the right) must also be given default arguments.
    - **Default arguments can not be redeclared, and must be declared before use**
        - The default argument must also be declared in the translation unit before it can be used
        - If the function has a forward declaration (especially one in a header file), put the default argument there. Otherwise, put the default argument in the function definition.
    - Default values are not part of a function's signature, so these function declarations are differentiated overloads.
        
        ```cpp
        void print(int x);                  // signature print(int)
        void print(int x, int y = 10);      // signature print(int, int)
        void print(int x, double y = 20.5); // signature print(int, double)
        ```
        
        - However, any call using only one argument (e.g. `print(5)`) will fail to compile, because the compiler cannot decide between `print(int, int)` and `print(int, double)` when their second parameters are both filled by defaults. Only explicit two-argument calls like `print(5, 10)` or `print(5, 20.5)` are unambiguous.
    - **Default arguments and function overloading**
        
        ```cpp
        void print(std::string_view s)
        {
            std::cout << s << '\n';
        }
        
        void print(char c = ' ')
        {
            std::cout << c << '\n';
        }
        ```
        
    - **Default arguments don't work for functions called through function pointers**
- **[[Function Templates]]**
    - Instead of manually creating a bunch of mostly-identical functions or classes (one for each set of different types), we instead create a single *template*.
    - A **template** definition describes what a function or class looks like.
        - Once a template is defined, the compiler can use the template to generate as many overloaded functions (or classes) as needed, each using different actual types!
        - Templates can work with types that didn't even exist when the template was written
    - C++ supports 3 different kinds of template parameters:
        - Type template parameters (where the template parameter represents a type).
        - Non-type template parameters (where the template parameter represents a constexpr value).
        - Template template parameters (where the template parameter represents a template).
    - **Function templates**
        - A function-like definition that is used to generate one or more overloaded functions, each with a different set of actual types.
        - The initial function template that is used to generate other functions is called the **primary template**, and the functions generated from the primary template are called **instantiated functions**.
        - When we create a primary function template, we use **placeholder types** (technically called **type template parameters**, informally called **template types**) for any parameter types, return types, or types used in the function body that we want to be specified later, by the user of the template.
        
        ```cpp
        template <typename T> // this is the template parameter declaration defining T as a type template parameter
        T max(T x, T y) // this is the function template definition for max<T>
        {
            return (x < y) ? y : x;
        }
        ```
        
        - **Function template instantiation**
            - To use the function template above: `max<actual_type>(arg1, arg2); // actual_type is some actual type, like int or double`
            - When a function is instantiated due to a function call, it's called **implicit instantiation**.
            - A function that is instantiated from a template is technically called a **specialization**, but in common language is often called a **function instance**.
            - A function template is only instantiated the first time a function call is made in each translation unit. Further calls to the function are routed to the already instantiated function.
        - **Template argument deduction**
            - Have the compiler deduce the actual type that should be used from the argument types in the function call.
            - When the compiler is doing template argument deduction, it won't do any type conversions.
            - Following the example above, either works: `max(1, 2)` or `max<>(1, 2)` .
                - In the first case (with no angled brackets), the compiler will consider both `max<int>` template function overloads and `max` non-template function overloads.
                - In the second case (with the empty angled brackets), the compiler will only consider `max<int>` template function overloads when determining which overloaded function to call.
                - Favor the normal function call syntax when making calls to a function instantiated from a function template (unless you need the function template version to be preferred over a matching non-template function).
        - **Function templates with non-template parameters**
            
            ```cpp
            template <typename T>
            int someFcn(T, double)
            {
                return 5;
            }
            ```
            
            - This function template has a templated first parameter, but the second parameter is fixed with type `double` .
        - **Instantiated functions may not always make sense semantically**
            - We can tell the compiler that instantiation of function templates with certain arguments should be disallowed.
                - `const char* addOne(const char* x) = delete;`
        - **Function templates with multiple template type parameters**
            
            ```cpp
            template <typename T, typename U> // We're using two template type parameters named T and U
            T max(T x, U y) // x can resolve to type T, and y can resolve to type U
            {
                return (x < y) ? y : x; // uh oh, we have a narrowing conversion problem here
            }
            ```
            
            - Because `T` and `U` are independent template parameters, they resolve their types independent of each other. This means `T` and `U` can resolve to different types, or they can resolve to the same type.
            - But the results undergo narrow conversion, so we can use this type:
            
            ```cpp
            template <typename T, typename U>
            auto max(T x, U y) // ask compiler can figure out what the relevant return type is
            {
                return (x < y) ? y : x;
            }
            ```
            
            - If we need a function that can be forward declared, we have to be explicit about the return type
        - **Abbreviated function templates**
            - C++20: When the `auto` keyword is used as a parameter type in a normal function, the compiler will automatically convert the function into a function template with each auto parameter becoming an independent template type parameter.
            
            ```cpp
            auto max(auto x, auto y)
            {
                return (x < y) ? y : x;
            }
            ```
            
            - Same as:
            
            ```cpp
            template <typename T, typename U>
            auto max(T x, U y)
            {
                return (x < y) ? y : x;
            }
            ```
            
        - **Function templates may be overloaded**
            
            ```cpp
            // Add two values with matching types
            template <typename T>
            auto add(T x, T y)
            {
                return x + y;
            }
            
            // Add two values with non-matching types
            // As of C++20 we could also use auto add(auto x, auto y)
            template <typename T, typename U>
            auto add(T x, U y)
            {
                return x + y;
            }
            
            // Add three values with any type
            // As of C++20 we could also use auto add(auto x, auto y, auto z)
            template <typename T, typename U, typename V>
            auto add(T x, U y, V z)
            {
                return x + y + z;
            }
            ```
            
            - Whichever function template is more restrictive/specialized will be preferred.
    - Template types are sometimes called **generic types**
    - Use function templates to write generic code that can work with a wide variety of types whenever you have the need.
    - **[[Non-type Template Parameters]]**
        - A **non-type template parameter** is a template parameter with a fixed type that serves as a placeholder for a constexpr value passed in as a template argument.
        
        ```cpp
        template <int N> // declare a non-type template parameter of type int named N
        void print()
        {
            std::cout << N << '\n'; // use value of N here
        }
        
        print<5>(); // 5 is our non-type template argument
        ```
        
        - Non-type template parameters are used primarily when we need to pass constexpr values to functions (or class types) so they can be used in contexts that require a constant expression.
        - **Implicit conversions for non-type template arguments**
            - In this context, only certain types of constexpr conversions are allowed. The most common types of allowed conversions include:
                - Integral promotions (e.g. `char` to `int`)
                - Integral conversions (e.g. `char` to `long` or `int` to `char`)
                - User-defined conversions (e.g. some program-defined class to `int`)
                - Lvalue to rvalue conversions (e.g. some variable `x` to the value of `x`)
        - As of C++17, non-type template parameters may use `auto` to have the compiler deduce the non-type template parameter from the template argument
        
        ```cpp
        template <auto N> // deduce non-type template parameter from template argument
        void print()
        {
            std::cout << N << '\n';
        }
        ```
        
    - **Using function templates in multiple files**
        - The most conventional way to address this issue is to put all your template code in a header (.h) file instead of a source (.cpp) file
        - Template definitions are exempt from the part of the one-definition rule that requires only one definition per program, so it is not a problem to have the same template definition #included into multiple source files.
            - Functions implicitly instantiated from function templates are implicitly inline, so they can be defined in multiple files, so long as each definition is identical.
