---
tags:
  - cpp/functions
  - cpp/lambdas
  - cpp/memory
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Advanced Functions
  - Ch20
up: LearnCPP
related:
  - "[[Function Pointers]]"
  - "[[The Stack and the Heap]]"
  - "[[Recursion]]"
  - "[[Command Line Arguments]]"
  - "[[Ellipsis]]"
  - "[[Lambdas]]"
  - "[[Lambda Captures]]"
---

# Chapter 20 — Functions

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 8, 2026 12:09 PM

## Notes

- **[[Function Pointers]]**
    
    ```cpp
    // function prototypes
    int foo();
    double goo();
    int hoo(int x);
    
    // function pointer initializers
    int (*fcnPtr1)(){ &foo };    // okay
    int (*fcnPtr2)(){ &goo };    // wrong -- return types don't match!
    double (*fcnPtr4)(){ &goo }; // okay
    fcnPtr1 = &hoo;              // wrong -- fcnPtr1 has no parameters, but hoo() does
    int (*fcnPtr3)(int){ &hoo }; // okay
    ```
    
    - Unlike fundamental types, C++ *will* implicitly convert a function into a function pointer if needed
    - However, function pointers will not convert to void pointers, or vice-versa
    - Function pointers can also be initialized or assigned the value nullptr
    - Could call function by dereferencing manually or implicit dereference:
    
    ```cpp
        int (*fcnPtr)(int){ &foo }; // Initialize fcnPtr with function foo
        fcnPtr(5); // call function foo(5) through fcnPtr.
    ```
    
    - **Default arguments don't work for functions called through function pointers**
        - Because the resolution happens at runtime, default arguments are not resolved when a function is called through a function pointer.
        - This means that we can use a function pointer to disambiguate a function call that would otherwise be ambiguous due to default arguments.
    - Functions used as arguments to another function are sometimes called **callback functions**.
    
    ```cpp
    // Default the sort to ascending sort
    void selectionSort(int* array, int size, bool (*comparisonFcn)(int, int) = ascending);
    ```
    
    - **Using std::function**
        
        ```cpp
        #include <functional>
        bool validate(int x, int y, std::function<bool(int, int)> fcn); // std::function method that returns a bool and takes two int parameters
        ```
        
    - **Type inference for function pointers**
        - the *auto* keyword can also infer the type of a function pointer.
- **[[The Stack and the Heap]]**
    - The memory that a program uses is typically divided into a few different areas, called segments:
        - The code segment (also called a text segment), where the compiled program sits in memory. The code segment is typically read-only.
        - The bss segment (also called the uninitialized data segment), where zero-initialized global and static variables are stored.
        - The data segment (also called the initialized data segment), where initialized global and static variables are stored.
        - The heap, where dynamically allocated variables are allocated from.
        - The call stack, where function parameters, local variables, and other function-related information are stored.
    - **The heap segment**
        - The heap has advantages and disadvantages:
            - Allocating memory on the heap is comparatively slow.
            - Allocated memory stays allocated until it is specifically deallocated (beware memory leaks) or the application ends (at which point the OS should clean it up).
            - Dynamically allocated memory must be accessed through a pointer. Dereferencing a pointer is slower than accessing a variable directly.
            - Because the heap is a big pool of memory, large arrays, structures, or classes can be allocated here.
    - **The call stack**
        - The call stack keeps track of all the active functions (those that have been called but have not yet terminated) from the start of the program to the current point of execution, and handles allocation of all function parameters and local variables.
        - When the application starts, the main() function is pushed on the call stack by the operating system. Then the program begins executing.
        - The sequence of steps that takes place when a function is called:
            1. The program encounters a function call.
            2. A stack frame is constructed and pushed on the stack. The stack frame consists of:
                - The address of the instruction beyond the function call (called the **return address**). This is how the CPU remembers where to return to after the called function exits.
                - All function arguments.
                - Memory for any local variables
                - Saved copies of any registers modified by the function that need to be restored when the function returns
            3. The CPU jumps to the function's start point.
            4. The instructions inside of the function begin executing.
        - When the function terminates, the following steps happen:
            1. Registers are restored from the call stack
            2. The stack frame is popped off the stack. This frees the memory for all local variables and arguments.
            3. The return value is handled.
            4. The CPU resumes execution at the return address.
        - Return values can be handled in a number of different ways, depending on the computer's architecture. Some architectures include the return value as part of the stack frame. Others use CPU registers.
    - **Stack overflow** happens when all the memory in the stack has been allocated -- in that case, further allocations begin overflowing into other sections of memory.
    - The stack has advantages and disadvantages:
        - Allocating memory on the stack is comparatively fast.
        - Memory allocated on the stack stays in scope as long as it is on the stack. It is destroyed when it is popped off the stack.
        - All memory allocated on the stack is known at compile time. Consequently, this memory can be accessed directly through a variable.
        - Because the stack is relatively small, it is generally not a good idea to do anything that eats up lots of stack space. This includes allocating or copying large arrays or other memory-intensive structures.
- **[[Recursion]]**
    - A **tail call** is a function call that occurs at the tail (end) of a function.
        - Functions with recursive tail calls are fairly easy for the compiler to optimize into an iterative (non-recursive) function.
    - One technique, called **memoization**, caches the results of expensive function calls so the result can be returned when the same input occurs again.
- **[[Command Line Arguments]]**
    - **Command line arguments** are optional string arguments that are passed by the operating system to the program when it is launched.
    - **Passing command line arguments**
        - In order to pass command line arguments to WordCount, we simply list the command line arguments after the executable name:
            - `WordCount Myfile.txt`
        
        ```jsx
        int main(int argc, char* argv[])
        //or
        int main(int argc, char** argv)
        ```
        
        - **argc** is an integer parameter containing a count of the number of arguments passed to the program
        - **argv** is where the actual argument values are stored (think: argv = **arg**ument **v**alues, though the proper name is "argument vectors")
            - argv is really just a C-style array of char pointers (each of which points to a C-style string).
        - Command line arguments are always passed as strings, even if the value provided is numeric in nature.
            - Conversion:
            
            ```cpp
            std::stringstream convert{ argv[1] }; // set up a stringstream variable named convert, initialized with the input from argv[1]
            
            	int myint{};
            	if (!(convert >> myint)) // do the conversion
            		myint = 0; // if conversion fails, set myint to a default value
            ```
            
        - **The OS parses command line arguments first**
- **[[Ellipsis]]** (and why to avoid them)
    
    ```cpp
    // The ellipsis must be the last parameter
    // count is how many additional arguments we're passing
    double findAverage(int count, ...){    
    		
    		int sum{ 0 };

        // We access the ellipsis through a va_list, so let's declare one
        std::va_list list;

        // We initialize the va_list using va_start.  The first argument is
        // the list to initialize.  The second argument is the last non-ellipsis
        // parameter.
        va_start(list, count);

        // Loop through all the ellipsis values
        for (int arg{ 0 }; arg < count; ++arg)
        {
             // We use va_arg to get values out of our ellipsis
             // The first argument is the va_list we're using
             // The second argument is the type of the value
             sum += va_arg(list, int);
        }

        // Cleanup the va_list when we're done.
        va_end(list);

        return static_cast<double>(sum) / count;
     }
    ```
    
    - **Why ellipsis are dangerous: Type checking is suspended**
        - With regular function parameters, the compiler uses type checking to ensure the types of the function arguments match the types of the function parameters (or can be implicitly converted so they match).
        - When using ellipsis, the compiler completely suspends type checking for ellipsis parameters.
    - **Why ellipsis are dangerous: ellipsis don't know how many parameters were passed**
        - **Method 1: Pass a length parameter**
        - **Method 2: Use a sentinel value**
            - A **sentinel** is a special value that is used to terminate a loop when it is encountered.
        - **Method 3: Use a decoder string**
        
        ```cpp
        	va_start(list, decoder);

        	for (auto codetype: decoder)
        	{
        		switch (codetype)
        		{
        		case 'i':
        			sum += va_arg(list, int);
        			break;

        		case 'd':
        			sum += va_arg(list, double);
        			break;
        		}
        	}
        ```
        
    - if you do use ellipsis, it is better if all values passed to the ellipses parameter are the same type (e.g. all `int`, or all `double`, not a mix of each).
    - using a count parameter or decoder string parameter is generally safer than using a sentinel value.
- **[[Lambdas]]** (anonymous functions)
    - A **lambda expression** (also called a **lambda** or **closure**) allows us to define an anonymous function inside another function.
        - The nesting is important, as it allows us both to avoid namespace naming pollution, and to define the function as close to where it is used as possible (providing additional context).
    - Syntax:
        
        ```cpp
        [ captureClause ] ( parameters ) -> returnType
        {
            statements;
        }
        
        [] {}; // a lambda with an omitted return type, no captures, and omitted parameters.
        
          // Define the function right where we use it.
        auto found{ std::find_if(arr.begin(), arr.end(),
                   [](std::string_view str) // here's our lambda, no capture clause
                  {
                    return str.find("nut") != std::string_view::npos;
                   }) };
        ```
        
        - The capture clause can be empty if no captures are needed.
        - The parameter list can be empty if no parameters are required. It can also be omitted entirely unless a return type is specified.
        - The return type is optional, and if omitted, `auto` will be assumed (thus using type deduction used to determine the return type).
    - lambdas are preferred over normal functions when we need a trivial, one-off function to pass as an argument to some other function.
    - This use of a lambda is sometimes called a **function literal**.
    - In actuality, lambdas aren't functions (which is part of how they avoid the limitation of C++ not supporting nested functions). They're a special kind of object called a functor. Functors are objects that contain an overloaded `operator()` that make them callable like a function.
    - Storing a lambda in a variable provides a way for us to give the lambda a useful name, which can help make our code more readable.
        - Storing a lambda in a variable also provides us with a way to use that lambda more than once.
    - The only way of using the lambda's actual type is by means of `auto`. (C++ 20)
        - Otherwise, use a function with a type template parameter or `std::function` parameter (or a function pointer if the lambda has no captures).
    - **Generic lambdas**
        - since C++14 we're allowed to use `auto` for parameters (note: in C++20, regular functions are able to use `auto` for parameters too)
    - **Constexpr lambdas**
        - As of C++17, lambdas are implicitly constexpr if the result satisfies the requirements of a constant expression.
        - The lambda must either have no captures, or all captures must be constexpr.
        - The functions called by the lambda must be constexpr. Note that many standard library algorithms and math functions weren't made constexpr until C++20 or C++23.
    - Note that if the generic lambda uses static duration variables, those variables are not shared between the generated lambdas.
    - **Return type deduction and trailing return types**
        - If return type deduction is used, a lambda's return type is deduced from the `return`-statements inside the lambda, and all return statements in the lambda must return the same type (otherwise the compiler won't know which one to prefer).
    - **Standard library function objects**
        
        ```cpp
         #include <functional> // for std::greater
         // Pass std::greater to std::sort
          std::sort(arr.begin(), arr.end(), std::greater{}); // note: need curly braces to instantiate object
        ```
        
    - **[[Lambda Captures]]**
        - **Capture clauses and capture by value**
            - Unlike nested blocks, where any identifier accessible in the outer block is accessible in the nested block, lambdas can only access certain kinds of objects that have been defined outside the lambda.
        - **The capture clause**
            - The **capture clause** is used to (indirectly) give a lambda access to variables available in the surrounding scope that it normally would not have access to.
            
            ```cpp
              std::string search{};
              std::cin >> search;
            
              // Capture @search                                vvvvvv
              auto found{ std::find_if(arr.begin(), arr.end(), [search](std::string_view str) {
                return str.find(search) != std::string_view::npos;
              }) };
            ```
            
        - When a lambda definition is executed, for each variable that the lambda captures, a clone of that variable is made (with an identical name) inside the lambda. These cloned variables are initialized from the outer scope variables of the same name at this point.
        - When the compiler encounters a lambda definition, it creates a custom object definition for the lambda. Each captured variable becomes a data member of the object.
            - At runtime, when the lambda definition is encountered, the lambda object is instantiated, and the members of the lambda are initialized at that point.
        - **Captures are treated as const by default**
            - **Mutable captures**
                - To allow modifications of variables that were captured, we can mark the lambda as `mutable`:
                
                ```cpp
                auto shoot{
                    [ammo]() mutable { // now mutable
                      // We're allowed to modify ammo now
                      --ammo;
                
                      std::cout << "Pew! " << ammo << " shot(s) left.\n";
                    }
                  };
                ```
                
        - **Capture by reference**
            
            ```cpp
            auto shoot{
                // We don't need mutable anymore
                [&ammo]() { // &ammo means ammo is captured by reference
                  // Changes to ammo will affect main's ammo
                  --ammo;
            
                  std::cout << "Pew! " << ammo << " shot(s) left.\n";
                }
            ```
            
            - Unlike variables that are captured by value, variables that are captured by reference are non-const, unless the variable they're capturing is `const`.
                - Capture by reference should be preferred over capture by value whenever you would normally prefer passing an argument to a function by reference (e.g. for non-fundamental types).
            - capturing by reference allows lambdas to modify captures without mutable
        - Multiple variables can be captured by separating them with a comma. This can include a mix of variables captured by value or by reference
        - **Default captures**
            - A **default capture** (also called a **capture-default**) captures all variables that are mentioned in the lambda.
            - Variables not mentioned in the lambda are not captured if a default capture is used.
            - To capture all used variables by value, use a capture value of =.
            - To capture all used variables by reference, use a capture value of &.
            - Default captures can be mixed with normal captures. We can capture some variables by value and others by reference, but each variable can only be captured once.
            
            ```cpp
            // Capture health and armor by value, and enemies by reference.
            [health, armor, &enemies](){};
            
            // Capture enemies by reference and everything else by value.
            [=, &enemies](){};
            
            // Capture armor by value and everything else by reference.
            [&, armor](){};
            
            // Illegal, we already said we want to capture everything by reference.
            [&, &armor](){};
            
            // Illegal, we already said we want to capture everything by value.
            [=, armor](){};
            
            // Illegal, armor appears twice.
            [armor, &health, &armor](){};
            
            // Illegal, the def
            ```
            
        - **Defining new variables in the lambda-capture**
            
            ```cpp
            // Declare a new variable that's visible only to the lambda.
            // The type of userArea is automatically deduced to int.
            [userArea{ width * height }](int knownArea) {
                return userArea == knownArea;
            }) };
            ```
            
            - Only initialize variables in the capture if their value is short and their type is obvious. Otherwise it's best to define the variable outside of the lambda and capture it.
        - **Dangling captured variables**
            - If a variable captured by reference dies before the lambda, the lambda will be left holding a dangling reference.
        - **Unintended copies of mutable lambdas**
            - Because lambdas are objects, they can be copied.
            
            ```cpp
              count(); // invoke count
            
              auto otherCount{ count }; // create a copy of count
            
              // invoke both count and the copy
              count();
              otherCount();
            ```
            
            - If we need to pass a mutable lambda, and want to avoid the possibility of inadvertent copies being made, there are two options.
                - use a non-capturing lambda instead
                - A better option is to prevent copies of our lambda from being made in the first place.
                    - One option (h/t to reader Dck) is to put our lambda into a `std::function` immediately. That way, when we call `myInvoke()`, the reference parameter `fn` can bind to our `std::function`, and no temporary copy is made
                    - An alternate solution is to use a reference wrapper.
                        - By wrapping our lambda in a `std::reference_wrapper`, whenever anybody tries to make a copy of our lambda, they'll make a copy of the reference_wrapper instead (avoiding making a copy of the lambda).
            - Standard library functions may copy function objects (reminder: lambdas are function objects). If you want to provide lambdas with mutable captured variables, pass them by reference using `std::ref`.
            - Try to avoid mutable lambdas. Non-mutable lambdas are easier to understand and don't suffer from the above issues, as well as more dangerous issues that arise when you add parallel execution.
