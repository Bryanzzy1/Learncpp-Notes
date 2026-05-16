# Chapter 12 — References and Pointers

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 16, 2026 3:31 PM

## Notes

- **Compound data types**
    - Types that are defined in terms of other existing data types.
    - A **class type** is a type that is a struct, class, or union.
    - **Value categories (lvalues and rvalues)**
        - An **lvalue** (short for “left value” or “locator value”, and sometimes written as “l-value”) is an expression that evaluates to an identifiable object or function (or bit-field).
            - lvalues come in two subtypes: a **modifiable lvalue** is an lvalue whose value can be modified. A **non-modifiable lvalue** is an lvalue whose value can’t be modified (because the lvalue is const or constexpr).
            - An entity (such as an object or function) that has an identity can be differentiated from other similar entities (typically by comparing the addresses of the entity).
            - Entities with identities can be accessed via an identifier, reference, or pointer, and typically have a lifetime longer than a single expression or statement.
        - An **rvalue** (short for “right value”, and sometimes written as `r-value`) is an expression that is not an lvalue.
            - Rvalue expressions evaluate to a value.
                - literals and the return value of functions and operators that return by value.
                - Rvalues aren’t identifiable (meaning they have to be used immediately), and only exist within the scope of the expression in which they are used.
            - Unless otherwise specified, operators expect their operands to be rvalues.
        - **Lvalue-to-rvalue conversion**
            - In cases where an rvalue is expected but an lvalue is provided, the lvalue will undergo an lvalue-to-rvalue conversion so that it can be used in such contexts.
                - The lvalue is evaluated to produce its value, which is an rvalue.
        - A C-style string literal is an lvalue because C-style strings (which are C-style arrays) decay to a pointer.
        - **Lvalue references**
            - A **reference** is an alias for an existing object
            - Once a reference has been defined, any operation on the reference is applied to the object being referenced.
            
            ```cpp
            int        // a normal int type (not an reference)
            int&       // an lvalue reference to an int object
            double&    // an lvalue reference to a double object
            const int& // an lvalue reference to a const int object
            ```
            
            - A type that specifies a reference (e.g. `int&`) is called a **reference type**. The type that can be referenced (e.g. `int`) is called the **referenced type.**
                - An lvalue reference to a non-const is commonly just called an “lvalue reference”, but may also be referred to as an **lvalue reference to non-const** or a **non-const lvalue reference** (since it isn’t defined using the `const` keyword).
                - An lvalue reference to a const is commonly called either an **lvalue reference to const** or a **const lvalue reference**.
            - An **lvalue reference variable** is a variable that acts as a reference to an lvalue (usually another variable).
            
            ```cpp
            int main()
            {
                int x { 5 }; // normal integer variable
                int& ref { x }; // ref is now an alias for variable x
            
                std::cout << x << ref << '\n'; // print 55
            
                x = 6; // x now has value 6
            
                std::cout << x << ref << '\n'; // prints 66
            
                ref = 7; // the object being referenced (x) now has value 7
            
                std::cout << x << ref << '\n'; // prints 77
            
                return 0;
            }
            ```
            
            - A bit different from pointers
            - References are initialized using a form of initialization called **reference initialization**.
                - When a reference is initialized with an object (or function)(**referent)**, we say it is **bound** to that object (or function).
                - **reference binding**
            - Non-const lvalue references can only be bound to a *modifiable* lvalue
                - non-const lvalue reference can't bind to an rvalue
            - In most cases, a reference will only bind to an object whose type matches the referenced type, (there are some exceptions to this rule that we’ll discuss when we get into inheritance).
            - **References can’t be reseated**
            - With one exception, the lifetime of a reference and the lifetime of its referent are independent.
                - A reference can be destroyed before the object it is referencing.
                - The object being referenced can be destroyed before the reference.
            - When an object being referenced is destroyed before a reference to it, the reference is left referencing an object that no longer exists.  (**Dangling reference)**
            - **References aren’t objects**
            - Aside:
                
                ```cpp
                int var{};
                int& ref1{ var };  // an lvalue reference bound to var
                int& ref2{ ref1 }; // an lvalue reference bound to var
                ```
                
                - ref2 is not a reference to a reference but the same reference as ref1 bounded to var.
            - **Lvalue references to const**
                - By using the `const` keyword when declaring an lvalue reference, we tell an lvalue reference to treat the object it is referencing as const.
                    - a **reference to const** or a **const reference**
                - Favor `lvalue references to const` over `lvalue references to non-const` unless you need to modify the object being referenced.
                - lvalues references to const can also bind to rvalues
                    - `const int& ref { 5 }; // okay: 5 is an rvalue`
                - Lvalue references to const can even bind to values of a different type, so long as those values can be implicitly converted to the reference type
                    - When a reference is bound to a temporary copy of the object or a temporary resulting from the conversion of the object, instead
                        - Temporary object are created when conversion happens
                        
                        ```cpp
                        const double& r1 { 5 };  // temporary double initialized with value 5, r1 binds to temporary
                        
                        const int& r2 { c };     // temporary int initialized with value 'a', r2 binds to temporary
                        ```
                        
                        - Any modifications subsequently made to the original object will not be seen by the reference (as it is referencing a different object), and vice-versa.
                        - When a const lvalue reference is *directly* bound to a temporary object, the lifetime of the temporary object is extended to match the lifetime of the reference.
                - When applied to a reference, `constexpr` allows the reference to be used in a constant expression.
                    - They can only be bound to objects with static duration (either globals or static locals).
                    - A constexpr reference cannot bind to a (non-static) local variable because the address of local variables is not known until the function they are defined within is actually called.
                    - Typically don’t see much use.
            - **Pass by lvalue reference**
                - Some objects are expensive to copy so we can avoid it by **Pass by reference**
                    - When using **pass by reference**, we declare a function parameter as a reference type (or const reference type) rather than as a normal type.
                    - Pass by reference allows us to pass arguments to a function without making copies of those arguments each time the function is called.
                    - **Pass by reference allows us to change the value of an argument**
                - **Pass by reference can only accept modifiable lvalue arguments**
            - **Pass by const lvalue reference**
                - Unlike a reference to non-const (which can only bind to modifiable lvalues), a reference to const can bind to modifiable lvalues, non-modifiable lvalues, and rvalues.
                - Favor passing by const reference over passing by non-const reference unless you have a specific reason to do otherwise (e.g. the function needs to change the value of an argument).
                - With pass by reference, ensure the type of the argument matches the type of the reference, or it will result in an unexpected (and possibly expensive) conversion.
                - **When to use pass by value vs pass by reference**
                    - Fundamental types and enumerated types are cheap to copy, so they are typically passed by value.
                    - Class types can be expensive to copy (sometimes significantly so), so they are typically passed by const reference.
                    - As a rule of thumb, pass fundamental types by value and class types by const reference.
                - **The cost of pass by value vs pass by reference**
                    - With pass by value, initialization means making a copy.
                        - The cost of copying an object is generally proportional to two things:
                            - The size of the object. Objects that use more memory take more time to copy.
                            - Any additional setup costs. Some class types do additional setup when they are instantiated (e.g. such as opening a file or database, or allocating a certain amount of dynamic memory to hold an object of a variable size). These setup costs must be paid each time an object is copied.
                        - binding a reference to an object is always fast (about the same speed as copying a fundamental type)
                    - When setting up a function call, the compiler may be able to optimize by placing a reference or copy of a passed-by-value argument (if it is small in size) into a CPU register (which is fast to access) rather than into RAM (which is slower to access).
                        - Each time a value parameter is used, the running program can directly access the storage location (CPU register or RAM) of the copied argument.
                        - However, when a reference parameter is used, the running program must first directly access the storage location (CPU register or RAM) allocated to the reference to determine which object is being referenced.
                        - Only then can it access the storage location of the referenced object (in RAM).
                        - Therefore, each use of a value parameter is a single CPU register or RAM access, whereas each use of a reference parameter is a single CPU register or RAM access plus a second RAM access.A
                    - Compiler can sometimes optimize code that uses pass by value more effectively than code that uses pass by reference
                    - Why we don’t pass everything by reference:
                        - For objects that are cheap to copy, the cost of copying is similar to the cost of binding, but accessing the objects is faster and the compiler is likely to be able to optimize better.
                        - For objects that are expensive to copy, the cost of the copy dominates other performance considerations.
                    - An object is cheap to copy if it uses 2 or fewer “words” of memory (where a “word” is approximated by the size of a memory address, sizeof(T) <= 2 * sizeof(void*)) and it has no setup costs.
                    - Prefer passing strings using `std::string_view` (by value) instead of `const std::string&`, unless your function calls other functions that require C-style strings or `std::string` parameters.
- **Pointers**
    - The **address-of operator** (&) returns the memory address of its operand.
    - The **dereference operator** (*) (also occasionally called the **indirection operator**) returns the value at a given memory address as an lvalue
    - A **pointer** is an object that holds a *memory address* (typically of another variable) as its value.
        - In modern C++, the pointers we are talking about here are sometimes called “raw pointers” or “dumb pointers”, to help differentiate them from “smart pointers” that were introduced into the language more recently.
        - A type that specifies a pointer (e.g. `int*`) is called a **pointer type**.
        - Always initialize your pointers.
        - pointers are objects
        - References are “safe” (outside of dangling references), pointers are inherently dangerous
        - References can not be reseated (changed to reference something else), pointers can change what they are pointing at.
        - The size of a pointer is dependent upon the architecture the executable is compiled for -- a 32-bit executable uses 32-bit memory addresses -- consequently, a pointer on a 32-bit machine is 32 bits (4 bytes). With a 64-bit executable, a pointer would be 64 bits (8 bytes).
    - Value initialize your pointers (to be null pointers) if you are not initializing them with the address of a valid object.
        - the **nullptr** keyword represents a null pointer literal
        - `int* ptr { nullptr }; // can use nullptr to initialize a pointer to be a null pointer`
        - **Dereferencing a null pointer results in undefined behavior**
        - We can use `nullPtr==nullptr` to see if the pointer is a null pointer
        - A pointer should either hold the address of a valid object, or be set to nullptr
    - **Favor references over pointers whenever possible**
    - A **pointer to a const value** (sometimes called a `pointer to const` for short) is a (non-const) pointer that points to a constant value.
        - Just like a reference to const, a pointer to const can point to non-const variables too.
        - A pointer to const treats the value being pointed to as constant, regardless of whether the object at that address was initially defined as const or not
    - A **const pointer** is a pointer whose address can not be changed after initialization.
    
    ```cpp
    int *ptr; // pointer to int
    const int *ptr; // pointer to const int
    int *const ptr; // const pointer to int
    const int *const ptr; // const pointer to const int
    ```
    
    - **Pass by address**
        - With **pass by address**, instead of providing an object as an argument, the caller provides an object’s *address* (via a pointer).
        - Prefer pointer-to-const function parameters over pointer-to-non-const function parameters, unless the function needs to modify the object passed in.
        - Do not make function parameters const pointers unless there is some specific reason to do so.
        - Prefer pass by reference to pass by address unless you have a specific reason to use pass by address.
        - pass pointers by reference
            - `void nullify(int*& refptr) // refptr is now a reference to a pointer`
    - `nullptr` has type `std::nullptr_t` (defined in header <cstddef>)
    - **Don’t return non-const static local variables by reference**
    - **In and out parameters**
        - `void print(const std::string& s) // s is an in parameter`
        - A function parameter that is used only for the purpose of returning information back to the caller is called an **out parameter**.
- **Type deduction with pointers, references, and const**
    - In addition to dropping const, type deduction will also drop references
    - **Top-level const and low-level const**
        - A **top-level const** is a const qualifier that applies to an object itself.
        - A **low-level const** is a const qualifier that applies to the object being referenced or pointed to
    - **Type deduction and const references**
        - Dropping a reference may change a low-level const to a top-level const: `const std::string&` is a low-level const, but dropping the reference yields `const std::string`, which is a top-level const.
        - If you want a const reference, reapply the `const` qualifier even when it’s not strictly necessary, as it makes your intent clear and helps prevent mistakes.
    - **Type deduction and pointers**
        - Unlike references, type deduction does not drop pointers
- **std::optional**
    - C++17 introduces
    - A `std::optional<T>` can either have a value of type `T`, or not.
    
    | **Behavior** | **Pointer** | **`std::optional`** |
    | --- | --- | --- |
    | Hold no value | initialize/assign `{}` or `std::nullptr` | initialize/assign `{}` or `std::nullopt` |
    | Hold a value | initialize/assign an address | initialize/assign a value |
    | Check if has value | implicit conversion to bool | implicit conversion to bool or `has_value()` |
    | Get value | dereference | dereference or `value()` |
    
    ```cpp
    std::cout << *o1;             // dereference to get value stored in o1 (undefined behavior if o1 does not have a value)
    std::cout << o2.value();      // call value() to get value stored in o2 (throws std::bad_optional_access exception if o2 does not have a value)
    std::cout << o3.value_or(42); // call value_or() to get value stored in o3 (or value `42` if o3 doesn't have a value)
    ```
    
    - Return a `std::optional` (instead of a sentinel value) for functions that may fail, unless your function needs to return additional information about why it failed.
    - Prefer function overloading for optional function parameters (when possible). Otherwise, use `std::optional<T>` for optional arguments when `T` would normally be passed by value. Favor `const T*` when `T` is expensive to copy.
    - `std::expected` (introduced in C++23) is designed to handle the case where a function can return either an expected value or an unexpected error code.