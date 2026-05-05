---
tags:
  - cpp/functions
  - cpp/files
  - cpp/preprocessing
  - cpp/namespace
  - cpp/scope
  - concept
  - syntax
  - chapter
aliases:
  - Functions and Files
  - Ch2
up: LearnCPP
related:
  - "[[Functions]]"
  - "[[Pass by Value]]"
  - "[[Preprocessing]]"
  - "[[Namespaces]]"
  - "[[Header Files]]"
---
1
# Chapter 2 —  Functions and Files

Pin: No
Fav: No
Category: Done (https://www.notion.so/Done-24fb13b569b5836bb160816e5fa04870?pvs=21)
Created: April 13, 2026 8:45 PM
Last edited: April 17, 2026 2:27 PM

## Notes

- `main()`
    - `main()` is required to return an `int`.
    - Explicit function calls to `main()` are disallowed.
    - Global variables are initialized prior to the execution of `main`. If the initializer for such a variable invokes a function, then that function will execute prior to `main`
    - The C++ standard only defines the meaning of 3 status codes:
        - `0`
        - `EXIT_SUCCESS`
        - `EXIT_FAILURE`
            - `#include <cstdlib> // for EXIT_SUCCESS and EXIT_FAILURE`
        - Your `main` function should return the value `0` if the program ran normally.
            - The function `main()` will implicitly return the value `0` if no return statement is provided.
- **[[Pass by Value|Arguments and Parameters]]**
    - Value of each of the arguments is *copied* into the matching parameter (using [[Initialization|copy initialization]])
        - **pass by value**
        - **value parameters**
    - A parameter without a name is called an **unnamed parameter**
        
        ```cpp
        void doSomething(int/*count*/)//ok:unnamed parameter will not generate warning
        {
        }
        ```
        
- **[[Functions|Local Scopes]]**
    - Most often, local variables are created when the function is entered, and destroyed in the opposite order of creation when the function is exited.
    - If the object is a class type object, prior to destruction, a special function called a [[Destructors|destructor]] is invoked. (No cost in many cases)
        - At some point after destruction, the [[Memory Model|memory]] used by the object will be **deallocated** (freed up for reuse).
- [[Return by Value|Return by value]] returns a temporary object (that holds a copy of the return value) to the caller.
    - In modern C++ (especially C++17), the compiler will often skip creating the temporary and just initialize the variable directly with the return value.
- **[[Functions|Forward Declaration]]**
    - A **forward declaration** allows us to tell the compiler about the existence of an identifier *before* actually defining the identifier.
        - Forward declarations are used to tell the compiler about the existence of some function that has been defined in a different code file.
        
        ```cpp
        #include <iostream>
        
        // forward declaration of add() (using a function declaration)
        int add(int x, int y); 
        
        //Keep the parameter names in your function declarations.
        int add(int, int); // valid function declaration
        
        int main()
        {
        		// this works because we forward declared add() above
            std::cout << "The sum of 3 and 4 is: " << add(3, 4) << '\n';     return 0;
        }
        
        int add(int x, int y) // even though the body of add() isn't defined until here
        {
            return x + y;
        }
        ```
        
    - Summary table:
        
        
        | **Term** | **Technical Meaning** | **Examples** |
        | --- | --- | --- |
        | Declaration | Tells compiler about an identifier and its associated type information. | void foo(); // function forward declaration (no body)void goo() {}; // function definition (has body)int x; // variable definition |
        | Definition | Implements a function or instantiates a variable.Definitions are also declarations. | void foo() { } // function definition (has body)int x; // variable definition |
        | Pure declaration | A declaration that isn’t a definition. | void foo(); // function forward declaration (no body) |
        | Initialization | Provides an initial value for a defined object. | int x { 2 }; // x is initialized to value 2 |
    - **[[One Definition Rule]]**
- **Compiling Multiple Files**
    - Reader “geo” reports that you can have VS Code automatically compile all .cpp files in the directory by replacing `"${file}",` with `"${fileDirname}\\**.cpp"` (on Windows).
    - If you wish to be explicit about what files get compiled, replace `"${file}",` with the name of each file you wish to compile, one per line, like this: `"main.cpp", "add.cpp",`
- **[[Namespaces]]**
    - A name declared within a scope region (such as a namespace) is distinct from any identical name declared in another scope.
    - In C++, any name that is not defined inside a class, function, or a namespace is considered to be part of an implicitly-defined namespace called the **global namespace** (sometimes also called **the global scope**).
    - When you use an identifier that is defined inside a non-global namespace (e.g. the `std` namespace), you need to tell the compiler that the identifier lives inside the namespace.
    - When an identifier includes a namespace prefix, the identifier is called a **qualified name**.
    - Avoid using-directives (such as `using namespace std;`) at the top of your program or in header files.
- [[Preprocessing]]
    - All changes made by the preprocessor happen either temporarily in memory or using temporary files.
    - When the preprocessor has finished processing a code file, the result is called a **translation unit**.
    - **Preprocessor directives** (often just called *directives*) are instructions that start with a *#* symbol and end with a newline (NOT a semicolon).
        - The final output of the preprocessor contains no directives -- only the output of the processed directive is passed to the compiler.
        - When you *#include* a file, the preprocessor replaces the #include directive with the contents of the included file. The included contents are then preprocessed.
    - The *#define* directive can be used to create a macro. In C++, a **macro** is a rule that defines how input text is converted into replacement output text.
        - *Function-like macros*.
            - Act like functions, and serve a similar purpose. Their use is generally considered unsafe, and almost anything they can do can be done by a normal function.
        - *Object-like macros*
            
            ```cpp
            #define IDENTIFIER
            #define IDENTIFIER substitution_text
            ```
            
        - Macro names should be written in all uppercase letters, with words separated by underscores.
        - Avoid macros with substitution text unless no viable alternatives exist.
    - **[[Conditional Compilation]]**
        - *#ifdef*, *#ifndef*, and *#endif*.
            
            ```cpp
            #include <iostream>
            
            #define PRINT_JOE
            
            int main()
            {
            #ifdef PRINT_JOE
                std::cout << "Joe\n"; // will be compiled since PRINT_JOE is defined
            #endif
            
            #ifdef PRINT_BOB
                std::cout << "Bob\n"; // will be excluded since PRINT_BOB is not defined
            #endif
            
                return 0;
            }
            
            ```
            
            - *#ifndef* is the opposite of *#ifdef*
            - `#ifdef PRINT_BOB` = `#if defined(PRINT_BOB)`and `#ifndef PRINT_BOB` =`#if !defined(PRINT_BOB)`
            - *Use #if 0* to exclude a block of code from being compiled (as if it were inside a comment block)
            - # Doesn’t care about the scope where it was defined.
            - Directives are only valid from the point of definition to the end of the file in which they are defined.
- **[[Header Files]]**
    - Header files allow us to put declarations in one place and then import them wherever we need them
    - *Two Parts:*
        - *Header guard*
        - *The actual contents*
    - Basic diagram of including headers
        
    - When we use angled brackets, we’re telling the preprocessor that this is a header file we didn’t write ourselves.
    - For double quotes: The preprocessor will first search for the header file in the current directory.
        - If it can’t find a matching header there, it will then search the `include directories`.
    - Use the standard library header files without the .h extension. User-defined headers should still use a .h extension.
    - Tell your compiler or IDE that you have a bunch of header files in some other location, so that it will look there when it can’t find them in the current directory.
        - In your *tasks.json* configuration file, add a new line in the *“Args”* section:                                   `"-I./source/includes",`There is no space after the `-I`. For a full path (rather than a relative path), remove the `.` after `-I`.
    - Each file should explicitly #include all of the header files it needs to compile. Do not rely on headers included transitively from other headers.
    - **Order your #includes as follows (skipping any that are not relevant):**
        - The paired header file for this code file (e.g. `add.cpp` should `#include "add.h"`)
        - Other headers from the same project (e.g. `#include "mymath.h"`)
        - 3rd party library headers (e.g. `#include <boost/tuple/tuple.hpp>`)
        - Standard library headers (e.g. `#include <iostream>`)
    - **[[Header Files|Header Guards]]**
        
        ```cpp
        #ifndef SOME_UNIQUE_NAME_HERE
        #define SOME_UNIQUE_NAME_HERE
        
        // your declarations (and certain types of definitions) here
        
        #endif
        ```
        
        - `#pragma once` serves the same purpose as header guards: to avoid a header file from being included multiple times.
            - If a header file is copied so that it exists in multiple places on the file system, if somehow both copies of the header get included, header guards will successfully de-dupe the identical headers, but `#pragma once` won’t.
