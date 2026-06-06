---
tags:
  - cpp/memory
  - cpp/pointers
  - cpp/classes
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Dynamic Allocation
  - Ch19
up: LearnCPP
related:
  - "[[new and delete]]"
  - "[[Memory Leaks]]"
  - "[[Dynamic Array Allocation]]"
  - "[[RAII]]"
  - "[[Pointers to Pointers]]"
  - "[[Two-Dimensional Dynamic Arrays]]"
  - "[[Void Pointers]]"
---

# Chapter 19 — Dynamic Allocation

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 6, 2026 11:26 AM

## Notes

- **[[new and delete|Dynamic memory allocation with new and delete]]**
    - **Static memory allocation** happens for static and global variables. Memory for these types of variables is allocated once when your program is run and persists throughout the life of your program.
    - **Automatic memory allocation** happens for function parameters and local variables. Memory for these types of variables is allocated when the relevant block is entered, and freed when the block is exited, as many times as necessary.
        - Both static and automatic allocation have two things in common:
            - The size of the variable / array must be known at compile time.
            - Memory allocation and deallocation happens automatically (when the variable is instantiated / destroyed).
    - To allocate a *single* variable dynamically, we use the scalar (non-array) form of the **new** operator
        - **Operator new can fail**
    
    ```cpp
    new int; // dynamically allocate an integer (and discard the result)
    int* value { new (std::nothrow) int }; // value will be set to a null pointer if the integer allocation fails
    
    ```
    
    - In the above case, we're requesting an integer's worth of memory from the operating system. The new operator creates the object using that memory, and then returns a pointer containing the *address* of the memory that has been allocated.
    - Note that accessing heap-allocated objects is generally slower than accessing stack-allocated objects.
    - Unlike static or automatic memory, the program itself is responsible for requesting and disposing of dynamically allocated memory.
        - The allocation and deallocation for stack objects is done automatically.
    - When we are done with a dynamically allocated variable, we need to explicitly tell C++ to free the memory for reuse. For single variables, this is done via the scalar (non-array) form of the **delete** operator:
        
        ```cpp
        // assume ptr has previously been allocated with operator new
        delete ptr; // return the memory pointed to by ptr to the operating system
        ptr = nullptr; // set ptr to be a null pointer
        ```
        
        - Deleting a null pointer is okay, and does nothing. There is no need to conditionalize your delete statements.
        - Set deleted pointers to nullptr unless they are going out of scope immediately afterward.
    - **[[Memory Leaks]]**
        - Memory leaks happen when your program loses the address of some bit of dynamically allocated memory before giving it back to the operating system. The program can't delete the dynamically allocated memory, because it no longer knows where it is.
- **[[Dynamic Array Allocation|Dynamically allocating arrays]]**
    
    ```cpp
    int* array{ new int[length]{} }; // use array new.  Note that length does not need to be constant!
    
    delete[] array; // use array delete to deallocate array
    ```
    
    - When deleting a dynamically allocated array, we have to use the array version of delete, which is delete[].
        - tells the CPU that it needs to clean up multiple variables instead of a single variable
    - **Dynamic arrays are almost identical to fixed arrays**
    - Dynamically allocating an array allows you to set the array length at the time of allocation.
        - Consequently, we recommend avoiding doing this yourself. Use `std::vector` instead.
- **[[RAII|Destructors]]**
    - A **destructor** is another special kind of class member function that is executed when an object of that class is destroyed. Whereas constructors are designed to initialize a class, destructors are designed to help clean up.
    
    ```cpp
    ~IntArray() // destructor
    	{
    		// Dynamically delete the array we allocated earlier
    		delete[] m_array;
    	}
    ```
    
    - the destructor is called when an object is destroyed
    - **[[RAII]]**
        - (Resource Acquisition Is Initialization) is a programming technique whereby resource use is tied to the lifetime of objects with automatic duration (e.g. non-dynamically allocated objects).
        - A resource (such as memory, a file or database handle, etc…) is typically acquired in the object's constructor (though it can be acquired after the object is created if that makes sense).
        - The resource is released in the destructor, when the object is destroyed.
    - **Note that if you use the std::exit() function, your program will terminate and no destructors will be called.**
- **[[Pointers to Pointers|Pointers to pointers and dynamic multidimensional arrays]]**
    
    ```cpp
    int** ptrptr; // pointer to a pointer to an int, two asterisks
    ```
    
    - The most common use is to dynamically allocate an array of pointers
        - **[[Two-Dimensional Dynamic Arrays|Two-dimensional dynamically allocated arrays]]**
        
        ```cpp
        int** array { new int*[10] }; // allocate an array of 10 int pointers — these are our rows
        for (int count { 0 }; count < 10; ++count)
            array[count] = new int[5]; // these are our columns
        ```
        
        - Deallocation
        
        ```cpp
        for (int count { 0 }; count < 10; ++count)
            delete[] array[count];
        delete[] array; // this needs to be done last
        ```
        
- **[[Void Pointers]]**
    - also known as the generic pointer, is a special type of pointer that can be pointed at objects of any data type!
    - However, because the void pointer does not know what type of object it is pointing to, dereferencing a void pointer is illegal. Instead, the void pointer must first be cast to another pointer type before the dereference can be performed.
    - If you need to delete a void pointer, `static_cast` it back to the appropriate type first.
    - It is not possible to do pointer arithmetic on a void pointer.
    - Many other places where void pointers would once be used to handle multiple data types are now better done using templates, which also offer strong type checking.
