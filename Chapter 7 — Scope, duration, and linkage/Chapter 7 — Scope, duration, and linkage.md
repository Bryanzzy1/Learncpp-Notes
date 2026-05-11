---
tags:
  - cpp/scope
  - cpp/namespaces
  - cpp/linkage
  - cpp/variables
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Scope and Linkage
  - Ch7
up: LearnCPP
related:
  - "[[Block Scope]]"
  - "[[User-Defined Namespaces]]"
  - "[[Local Variables]]"
  - "[[Global Variables]]"
  - "[[Variable Shadowing]]"
  - "[[Internal Linkage]]"
  - "[[External Linkage]]"
  - "[[Inline Functions and Variables]]"
  - "[[Static Local Variables]]"
  - "[[Scope, Duration, and Linkage]]"
  - "[[Forward Declaration Summary]]"
  - "[[Storage Class Specifiers]]"
  - "[[Using Declarations and Directives]]"
  - "[[Unnamed and Inline Namespaces]]"
---

# Chapter 7 — Scope, duration, and linkage

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 7, 2026 4:19 PM

## Notes

- **[[Block Scope]]**
    - The majority of C++ standard says that C++ compilers should support 256 levels of nesting.
- **[[User-Defined Namespaces]]**
    - Namespaces that you create in your own programs are casually called **user-defined namespaces** (though it would be more accurate to call them **program-defined namespaces**).
        
        ```
        namespace NamespaceIdentifier
        {
            // content of namespace here
        }
        ```
        
    - Start namespace names with a capital letter.
    - Use :: for calling functions within a defined namespace
        - If an identifier inside a namespace is used and no scope resolution is provided, the compiler will first try to find a matching declaration in that same namespace. If no matching identifier is found, the compiler will then check each containing namespace in sequence to see if a match is found, with the global namespace being checked last.
        - It's legal to declare namespace blocks in multiple locations (either across multiple files, or multiple places within the same file). All declarations within the namespace are considered part of the namespace.
    - Namespaces can be nested inside other namespaces.
    - **Namespace alias**
        - `namespace Active = Foo::Goo; // active now refers to Foo::Goo`
- **[[Local Variables]]**
    - **Local variables have automatic storage duration**
        - For this reason, local variables are sometimes called **automatic variables**.
    - A variable's **storage duration** (usually just called **duration**) determines what rules govern when and how a variable will be created (instantiated) and destroyed.
    - An identifier's **linkage** determines whether a declaration of that same identifier in a different scope refers to the same object (or function).
- **[[Global Variables]]**
    - Prefer defining global variables inside a namespace rather than in the global namespace.
    - Global variables are created when the program starts (before `main()` begins execution), and destroyed when it ends. This is called **static duration**. Variables with *static duration* are sometimes called **static variables**.
        - Consider using a "g" or "g_" prefix when naming global variables (especially those defined in the global namespace), to help differentiate them from local variables and function parameters.
    - **(Non-const) global variables are evil**
        - The initialization order problem of global variables
            - The first phase is called **static initialization**. Static initialization proceeds in two phases:
                - Global variables with constexpr initializers (including literals) are initialized to those values. This is called **constant initialization**.
                - Global variables without initializers are zero-initialized. Zero-initialization is considered to be a form of static-initialization since `0` is a constexpr value.
            - The second phase is called **dynamic initialization**. This phase is more complex and nuanced, but the gist of it is that global variables with non-constexpr initializers are initialized.
            - Avoid initializing objects with static duration using other objects with static duration from a different translation unit.
            - Dynamic initialization of global variables is also susceptible to initialization order issues and should be avoided whenever possible.
- **[[Variable Shadowing]]**
    - We have a variable inside a nested block that has the same name as a variable in an outer block
    - Similar to how variables in a nested block can shadow variables in an outer block, local variables with the same name as a global variable will shadow the global variable wherever the local variable is in scope
    - Avoid variable shadowing.
- **[[Internal Linkage]]**
    - local variables have `no linkage`
    - Global variables and function identifiers can have either `internal linkage` or `external linkage`.
        - An identifier with **internal linkage** can be seen and used within a single translation unit, but it is not accessible from other translation units.
            - If two source files have identically named identifiers with internal linkage, those identifiers will be treated as independent
        - Global variables with internal linkage are sometimes called **internal variables**.
            - To make a non-constant global variable internal, we use the `static` keyword.
                - The use of the `static` keyword above is an example of a **storage class specifier**, which sets both the name's linkage and its storage duration.
            - Const and constexpr global variables have internal linkage by default.
    - Unnamed namespaces can give internal linkage to a wider range of identifiers (e.g. type identifiers), and they are better suited for giving many identifiers internal linkage.
- **[[External Linkage]]**
    - An identifier with **external linkage** can be seen and used both from the file in which it is defined, and from other code files (via a forward declaration).
    - **Functions have external linkage by default**
    - Global variables with external linkage are sometimes called **external variables**.
        - Non-constant globals have external linkage by default
        - To make a global variable external (and thus accessible by other files), we can use the `extern` keyword to do so.
            - Must place a `forward declaration` for the global variable in any other files wishing to use the variable.
            - `extern int g_x;       // this extern is a forward declaration of a variable named g_x that is defined somewhere else`
            - **Avoid using `extern` on a non-const global variable with an initializer**
- **[[Inline Functions and Variables]]**
    - All of the extra work that must happen to setup, facilitate, and/or cleanup after some task (in this case, making a function call) is called **overhead**.
    - **Inline expansion** is a process where a function call is replaced by the code from the called function's definition.
        - However, inline expansion has its own potential cost: if the body of the function being expanded takes more instructions than the function call being replaced, then each inline expansion will cause the executable to grow larger. Larger executables tend to be slower (due to not fitting as well in memory caches).
        - Every function falls into one of two categories, where calls to the function:
            - May be expanded (most functions are in this category).
            - Can't be expanded.
        - A function that is declared using the `inline` keyword is called an **inline function**.
            - Do not use the `inline` keyword to request inline expansion for your functions.
            - Premature optimization could hurt performance
            - In modern C++, the term `inline` has evolved to mean "multiple definitions are allowed". Thus, an inline function is one that is allowed to be defined in multiple translation units (without violating the ODR).
                - The compiler needs to be able to see the full definition of an inline function wherever it is used, and all definitions for an inline function (with external linkage) must be identical (or undefined behavior will result).
            - Inline functions are typically defined in header files, where they can be #included into the top of any code file that needs to see the full definition of the identifier.
    - If you need global constants and your compiler is C++17 capable, prefer defining inline constexpr global variables in a header file.
- **[[Static Local Variables]]**
    - Using the `static` keyword on a local variable changes its duration from automatic duration to static duration. This means the variable is now created at the start of the program, and destroyed at the end of the program (just like a global variable). As a result, the static variable will retain its value even after it goes out of scope!
    - Initialize your static local variables. Static local variables are only initialized the first time the code is executed, not on subsequent calls.
        - It's common to use "s_" to prefix static (static duration) local variables.
    - Static local variables can be made const (or constexpr).
        - Static local variables are best used to avoid expensive local object initialization each time a function is called.
    - Non-const static local variables should generally be avoided. If you do use them, ensure the variable never needs to be reset, and isn't used to alter program flow.
- **[[Scope, Duration, and Linkage]]**
    
    | **Type** | **Example** | **Scope** | **Duration** | **Linkage** | **Notes** |
    | --- | --- | --- | --- | --- | --- |
    | Local variable | int x; | Block | Automatic | None |  |
    | Static local variable | static int s_x; | Block | Static | None |  |
    | Dynamic local variable | int* x { new int{} }; | Block | Dynamic | None |  |
    | Function parameter | void foo(int x) | Block | Automatic | None |  |
    | Internal non-const global variable | static int g_x; | Global | Static | Internal | Initialized or uninitialized |
    | External non-const global variable | int g_x; | Global | Static | External | Initialized or uninitialized |
    | Inline non-const global variable (C++17) | inline int g_x; | Global | Static | External | Initialized or uninitialized |
    | Internal constant global variable | constexpr int g_x { 1 }; | Global | Static | Internal | Must be initialized |
    | External constant global variable | extern const int g_x { 1 }; | Global | Static | External | Must be initialized |
    | Inline constant global variable (C++17) | inline constexpr int g_x { 1 }; | Global | Static | External | Must be initialized |
- **[[Forward Declaration Summary]]**
    
    | **Type** | **Example** | **Notes** |
    | --- | --- | --- |
    | Function forward declaration | void foo(int x); | Prototype only, no function body |
    | Non-constant variable forward declaration | extern int g_x; | Must be uninitialized |
    | Const variable forward declaration | extern const int g_x; | Must be uninitialized |
    | Constexpr variable forward declaration | extern constexpr int g_x; | Not allowed, constexpr cannot be forward declared |
- **[[Storage Class Specifiers]]**
    
    | **Specifier** | **Meaning** | **Note** |
    | --- | --- | --- |
    | extern | static (or thread_local) storage duration and external linkage |  |
    | static | static (or thread_local) storage duration and internal linkage |  |
    | thread_local | thread storage duration |  |
    | mutable | object allowed to be modified even if containing class is const |  |
    | auto | automatic storage duration | Deprecated in C++11 |
    | register | automatic storage duration and hint to the compiler to place in a register | Deprecated in C++17 |
- **[[Using Declarations and Directives]]**
    - **Qualified and unqualified names**
        - A **qualified name** is a name that includes an associated scope.
        - An **unqualified name** is a name that does not include a scoping qualifier.
    - A **using declaration** allows us to use an unqualified name (with no scope) as an alias for a qualified name.
    - A **using directive** allows *all* identifiers in a given namespace to be referenced without qualification from the scope of the using-directive.
    - Difference:
        - Using Declaration: using std::cout
        - Using directive: using namespace std
    - **Do not use using-statements in header files, or before an #include directive**
    - Once a using-statement has been declared, there's no way to cancel or replace it with a different using-statement within the scope in which it was declared.
- **[[Unnamed and Inline Namespaces]]**
    - An **unnamed namespace** (also called an **anonymous namespace**) is a namespace that is defined without a name
        
        ```cpp
        namespace // unnamed namespace
        {
            void doSomething() // can only be accessed in this file
            {
                std::cout << "v1\n";
            }
        }
        
        int main()
        {
            doSomething(); // we can call doSomething() without a namespace prefix
        
            return 0;
        }
        ```
        
        - All identifiers inside an unnamed namespace are treated as if they have internal linkage, which means that the content of an unnamed namespace can't be seen outside of the file in which the unnamed namespace is defined.
            - For functions, this is effectively the same as defining all functions in the unnamed namespace as static functions.
        - Prefer unnamed namespaces when you have content you want to keep local to a translation unit.
        - Avoid unnamed namespaces in header files.
    - An **inline namespace** is a namespace that is typically used to version content.
    
    ```cpp
    #include <iostream>
    
    inline namespace V1 // declare an inline namespace named V1
    {
        void doSomething()
        {
            std::cout << "V1\n";
        }
    }
    
    namespace V2 // declare a normal namespace named V2
    {
        void doSomething()
        {
            std::cout << "V2\n";
        }
    }
    
    int main()
    {
        V1::doSomething(); // calls the V1 version of doSomething()
        V2::doSomething(); // calls the V2 version of doSomething()
    
        doSomething(); // calls the inline version of doSomething() (which is V1)
    
        return 0;
    }
    ```
    
    - A namespace can be both inline and unnamed
        - But it's probably better to nest an anonymous namespace inside an inline namespace.
