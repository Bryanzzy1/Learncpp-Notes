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
  - Move Semantics and Smart Pointers
  - Ch22
up: LearnCPP
related:
  - "[[Smart Pointers]]"
  - "[[Move Semantics]]"
  - "[[Move Constructor and Assignment]]"
  - "[[Rvalue References]]"
  - "[[std-move|std::move]]"
  - "[[std-unique-ptr|std::unique_ptr]]"
  - "[[std-shared-ptr|std::shared_ptr]]"
  - "[[std-weak-ptr|std::weak_ptr]]"
---

# Chapter 22 — Move Semantics and Smart Pointers

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 16, 2026 4:13 PM

## Notes

- **[[Smart Pointers]]**
    - A composition class that is designed to manage dynamically allocated memory and ensure that memory gets deleted when the smart pointer object goes out of scope.
    - 
- **[[Move Semantics]]**
    - the class will transfer ownership of the object rather than making a copy
    
    ```cpp
    // A copy constructor that implements move semantics
    	Auto_ptr2(Auto_ptr2& a) // note: not const
    	{
    		// We don't need to delete m_ptr here.  This constructor is only called when we're creating a new object, and m_ptr can't be set prior to this.
    		m_ptr = a.m_ptr; // transfer our dumb pointer from the source to our local object
    		a.m_ptr = nullptr; // make sure the source no longer owns the pointer
    	}
    	
    	
    	// An assignment operator that implements move semantics
    	Auto_ptr2& operator=(Auto_ptr2& a) // note: not const
    	{
    		if (&a == this)
    			return *this;
    
    		delete m_ptr; // make sure we deallocate any pointer the destination is already holding first
    		m_ptr = a.m_ptr; // then transfer our dumb pointer from the source to the local object
    		a.m_ptr = nullptr; // make sure the source no longer owns the pointer
    		return *this;
    	}
    ```
    
    - In C++11, the concept of "move" was formally defined, and "move semantics" were added to the language to properly differentiate copying from moving.
    - **[[Move Constructor and Assignment]]**
        
        ```cpp
        // Move constructor
        	// Transfer ownership of a.m_ptr to m_ptr
        	Auto_ptr4(Auto_ptr4&& a) noexcept
        		: m_ptr { a.m_ptr }
        	{
        		a.m_ptr = nullptr; // we'll talk more about this line below
        	}
        	
        	// Move assignment
        	// Transfer ownership of a.m_ptr to m_ptr
        	Auto_ptr4& operator=(Auto_ptr4&& a) noexcept
        	{
        		// Self-assignment detection
        		if (&a == this)
        			return *this;
        
        		// Release any resource we're holding
        		delete m_ptr;
        
        		// Transfer ownership of a.m_ptr to m_ptr
        		m_ptr = a.m_ptr;
        		a.m_ptr = nullptr; // we'll talk more about this line below
        
        		return *this;
        	}
        ```
        
        - Move constructors and move assignment should be marked as `noexcept`. This tells the compiler that these functions will not throw exceptions.
        - The move constructor and move assignment are called when those functions have been defined, and the argument for construction or assignment is an rvalue.
        - The copy constructor and copy assignment are used otherwise (when the argument is an lvalue, or when the argument is an rvalue and the move constructor or move assignment functions aren't defined).
        - **Implicit move constructor and move assignment operator**
            - The compiler will create an implicit move constructor and move assignment operator if all of the following are true:
                - There are no user-declared copy constructors or copy assignment operators.
                - There are no user-declared move constructors or move assignment operators.
                - There is no user-declared destructor.
        - If we construct an object or do an assignment where the argument is an r-value, then we know that r-value is just a temporary object of some kind. Instead of copying it (which can be expensive), we can simply transfer its resources (which is cheap) to the object we're constructing or assigning.
    - Move semantics is an optimization opportunity.
    - **Move functions should always leave both objects in a valid state**
    - **Disabling copying**
        - But in move-enabled classes, it is sometimes desirable to delete the copy constructor and copy assignment functions to ensure copies aren't made
        - You can delete the move constructor and move assignment using the `= delete` syntax in the exact same way you can delete the copy constructor and copy assignment.
            - The compiler will not generate an implicit move constructor
            - Makes the class not returnable by value in cases where mandatory copy elision does not apply.
    - **Issues with move semantics and `std::swap`**
        - Implementing the move constructor and move assignment using `std::swap()` is problematic, as `std::swap()` calls both the move constructor and move assignment on move-capable objects. This will result in an infinite recursion issue.
- **[[Rvalue References]]**
    
    ```cpp
    int x{ 5 };
    int& lref{ x }; // l-value reference initialized with l-value x
    int&& rref{ 5 }; // r-value reference initialized with r-value 5
    ```
    
    - r-value references extend the lifespan of the object they are initialized with to the lifespan of the r-value reference (l-value references to const objects can do this too).
    - non-const r-value references allow you to modify the r-value
    - when initializing an r-value reference with a literal, a temporary object is constructed from the literal so that the reference is referencing a temporary object, not a literal value.
    - **Rvalue reference variables are lvalues**
    - Almost never return an r-value reference, for the same reason you should almost never return an l-value reference.
- **[[std-move|std::move]]**
    - std::move is a standard library function that casts (using static_cast) its argument into an r-value reference, so that move semantics can be invoked.
    
    ```cpp
    	T tmp { std::move(a) }; // invokes move constructor
    	a = std::move(b); // invokes move assignment
    	b = std::move(tmp); // invokes move assignment
    ```
    
    - Only use `std::move()` on persistent objects whose value you want to move, and do not make any assumptions about the value of the object beyond that point.
    - There is a useful variant of `std::move()` called `std::move_if_noexcept()` that returns a movable r-value if the object has a `noexcept` move constructor, otherwise it returns a copyable l-value.
- **[[std-unique-ptr|std::unique_ptr]]**
    - the C++11 replacement for std::auto_ptr
    - It should be used to manage any dynamically allocated object that is not shared by multiple objects.
    - std::unique_ptr should completely own the object it manages, not share that ownership with other classes.
    - **Accessing the managed object**
        - std::unique_ptr has an overloaded operator* and operator-> that can be used to return the resource being managed.
        - Operator* returns a reference to the managed resource, and operator-> returns a pointer.
    - Favor std::array, std::vector, or std::string over a smart pointer managing a fixed array, dynamic array, or C-style string.
    - **std::make_unique C++14**
        - constructs an object of the template type and initializes it with the arguments passed into the function.
        - Use std::make_unique() instead of creating std::unique_ptr and using new yourself.
    - In general, you should not return std::unique_ptr by pointer (ever) or reference (unless you have a specific compelling reason to).
    - **Passing std::unique_ptr to a function**
        - Note that because copy semantics have been disabled, you'll need to use std::move to actually pass the variable in.
    - don't let multiple objects manage the same resource.
    
    ```cpp
    Resource* res{ new Resource() };
    std::unique_ptr<Resource> res1{ res };
    std::unique_ptr<Resource> res2{ res };
    ```
    
    - don't manually delete the resource out from underneath the std::unique_ptr.
    
    ```cpp
    Resource* res{ new Resource() };
    std::unique_ptr<Resource> res1{ res };
    delete res;
    ```
    
- **[[std-shared-ptr|std::shared_ptr]]**
    - meant to solve the case where you need multiple smart pointers co-owning a resource.
    - Internally, std::shared_ptr keeps track of how many std::shared_ptr are sharing the resource. As long as at least one std::shared_ptr is pointing to the resource, the resource will not be deallocated, even if individual std::shared_ptr are destroyed.
    - Always make a copy of an existing std::shared_ptr if you need more than one std::shared_ptr pointing to the same resource.
    - **std::make_shared**
        - std::make_shared() can (and should) be used to make a std::shared_ptr. std::make_shared() is available in C++11.
    - **Shared pointers can be created from unique pointers**
    - As of C++20, std::shared_ptr does have support for arrays.
    - std::shared_ptr is designed for the case where you need multiple smart pointers co-managing the same resource. The resource will be deallocated when the last std::shared_ptr managing the resource is destroyed.
- **[[std-weak-ptr|std::weak_ptr]]**
    - A **Circular reference** (also called a **cyclical reference** or a **cycle**) is a series of references where each object references the next, and the last object references back to the first, causing a referential loop.
    - It turns out, this cyclical reference issue can even happen with a single std::shared_ptr -- a std::shared_ptr referencing the object that contains it is still a cycle (just a reductive one).
    
    ```cpp
    	auto ptr1 { std::make_shared<Resource>() };

    	ptr1->m_ptr = ptr1; // m_ptr is now sharing the Resource that contains it
    ```
    
    - **std::weak_ptr**
        - std::weak_ptr was designed to solve the "cyclical ownership" problem described above.
        - it can observe and access the same object as a std::shared_ptr (or other std::weak_ptrs) but it is not considered an owner.
        - One downside of std::weak_ptr is that std::weak_ptr are not directly usable (they have no operator->).
            - To use a std::weak_ptr, you must first convert it into a std::shared_ptr. Then you can use the std::shared_ptr.
        - The easiest way to test whether a std::weak_ptr is valid is to use the `expired()` member function, which returns `true` if the std::weak_ptr is pointing to an invalid object, and `false` otherwise.
