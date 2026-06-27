---
tags:
  - cpp/error-handling
  - cpp/exceptions
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Exceptions
  - Ch27
up: LearnCPP
related:
  - "[[Basic Exception Handling]]"
  - "[[Stack Unwinding]]"
  - "[[Uncaught Exceptions]]"
  - "[[Catch-All Handler]]"
  - "[[Exception Classes]]"
  - "[[std-exception|std::exception]]"
  - "[[Rethrowing Exceptions]]"
  - "[[Function Try Blocks]]"
  - "[[Exception Dangers]]"
  - "[[noexcept]]"
  - "[[std-move-if-noexcept|std::move_if_noexcept]]"
---

# Chapter 27 — Exceptions

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 27, 2026 5:13 PM

## Notes

- **[[Basic Exception Handling]]**
    - In C++, a **throw statement** is used to signal that an exception or error case has occurred (think of throwing a penalty flag). Signaling that an exception has occurred is also commonly called **raising** an exception.
    
    ```cpp
    throw -1; // throw a literal integer value
    throw ENUM_INVALID_INDEX; // throw an enum value
    throw "Can not take square root of negative number"; // throw a literal C-style (const char*) string
    throw dX; // throw a double variable that was previously defined
    throw MyException("Fatal Error"); // Throw an object of class MyException
    ```
    
    - In C++, we use the **try** keyword to define a block of statements (called a **try block**). The try block acts as an observer, looking for any exceptions that are thrown by any of the statements within the try block.
    
    ```cpp
    try
    {
        // Statements that may throw exceptions you want to handle go here
        throw -1; // here's a trivial throw statement
    }
    ```
    
    - Actually handling exceptions is the job of the catch block(s). The **catch** keyword is used to define a block of code (called a **catch block**) that handles exceptions for a single data type.
    
    ```cpp
    catch (int x)
    {
        // Handle an exception of type int here
        std::cerr << "We caught an int exception with value" << x << '\n';
    }
    ```
    
    - When an exception is raised (using **throw**), the running program finds the nearest enclosing **try** block (propagating up the stack if necessary to find an enclosing try block -- we'll discuss this in more detail next lesson) to see if any of the **catch** handlers attached to the try block can handle that type of exception. If so, execution jumps to the top of the catch block, the exception is considered handled.
    - **Exceptions are handled immediately**
- **[[Stack Unwinding]]**
    - Try blocks catch exceptions not only from statements within the try block, but also from functions that are called within the try block.
    - Unwinding the stack destroys local variables in the functions that are unwound (which is good, because it ensures their destructors execute).
        - Keeping going up the call stack to see if any function can handle the throw
        - If a matching exception handler is found, then execution jumps from the point where the exception is thrown to the top of the matching catch block
- **[[Uncaught Exceptions]]**
    - When no exception handler for a function can be found, std::terminate() is called, and the application is terminated.
        - The call stack may or may not be unwound if an exception is unhandled.
        - If the stack is not unwound, local variables will not be destroyed, which may cause problems if those variables have non-trivial destructors.
    - **[[Catch-All Handler]]**
        
        ```cpp
        #include <iostream>
        
        int main()
        {
        	try
        	{
        		throw 5; // throw an int exception
        	}
        	catch (double x)
        	{
        		std::cout << "We caught an exception of type double: " << x << '\n';
        	}
        	catch (...) // catch-all handler
        	{
        		std::cout << "We caught an exception of an undetermined type\n";
        	}
        }
        ```
        
        - **Using the catch-all handler to wrap main()**
    - Debugging Unhandled exception
        
        ```cpp
        #ifndef NDEBUG// if we're in release node
            catch(...) // compile in the catch-all handler
            {
                std::cerr << "Abnormal termination\n";
            }
        #else // in debug mode, compile in a catch that will never be hit (for syntactic reasons)
            catch(DummyException)
            {
            }
        #endif
        ```
        
- **[[Exception Classes]]**
    - **Exceptions and member functions**
        - throw instead of assert
    - Constructors are another area of classes in which exceptions can be very useful.
        - class members are destructed even if the constructor fails, if you do the resource allocations inside the members of the class (rather than in the constructor itself), then those members can clean up after themselves when they are destructed.
    - **Exception classes**
        - a normal class that is designed specifically to be thrown as an exception.
        
        ```cpp
        class ArrayException
        {
        private:
        	std::string m_error;
        
        public:
        	ArrayException(std::string_view error)
        		: m_error{ error }
        	{
        	}
        
        	const std::string& getError() const { return m_error; }
        };
        ```
        
        - Exceptions of a fundamental type can be caught by value since they are cheap to copy.
        - Exceptions of a class type should be caught by (const) reference to prevent expensive copying and slicing.
    - **Exceptions and inheritance**
        - Handlers for derived exception classes should be listed before those for base classes.
    - **[[std-exception|std::exception]]**
        - a small interface class designed to serve as a base class to any exception thrown by the C++ standard library.
        
        ```cpp
            try
            {
                // Your code using standard library goes here
                std::string s;
                s.resize(std::numeric_limits<std::size_t>::max()); // will trigger a std::length_error or allocation exception
            }
            // This handler will catch std::exception and all the derived exceptions too
            catch (const std::exception& exception)
            {
                std::cerr << "Standard exception: " << exception.what() << '\n';
            }
        ```
        
        - **Using the standard exceptions directly**
            - std::runtime_error (included as part of the stdexcept header) is a popular choice, because it has a generic name, and its constructor takes a customizable message
            
            ```cpp
            int main()
            {
            	try
            	{
            		throw std::runtime_error("Bad things happened");
            	}
            	// This handler will catch std::exception and all the derived exceptions too
            	catch (const std::exception& exception)
            	{
            		std::cerr << "Standard exception: " << exception.what() << '\n';
            	}
            
            	return 0;
            }
            ```
            
            - std::runtime_error can take a C-style string parameter, or a `const std::string&` parameter.
    - When an exception is thrown, the object being thrown is typically a temporary or local variable that has been allocated on the stack.
        - When an exception is thrown, the compiler makes a copy of the exception object to some piece of unspecified memory (outside of the call stack) reserved for handling exceptions.
    - Exception objects need to be copyable.
    - Exception objects should not keep pointers or references to stack-allocated objects.
- **[[Rethrowing Exceptions]]**
    - Want to throw exceptions but don't want to fully handle at the point of catching
    - This is common when you want to log an error, but pass the issue along to the caller to actually handle.
    
    ```cpp
            catch (Base& b)
            {
                std::cout << "Caught Base b, which is actually a ";
                b.print();
                std::cout << '\n';
                throw; // note: We're now rethrowing the object here
            }
    ```
    
    - When rethrowing the same exception, use the throw keyword by itself
- **[[Function Try Blocks]]**
    - allow you to establish an exception handler around the body of an entire function, rather than around a block of code.
    - Use function try blocks when you need a constructor to handle an exception thrown in the member initializer list.
    
    ```cpp
    class A
    {
    private:
    	int m_x;
    public:
    	A(int x) : m_x{x}
    	{
    		if (x <= 0)
    			throw 1; // Exception thrown here
    	}
    };
    
    class B : public A
    {
    public:
    	B(int x) try : A{x} // note addition of try keyword here
    	{
    	}
    	catch (...) // note this is at same level of indentation as the function itself
    	{
                    // Exceptions from member initializer list or
                    // from constructor body are caught here
    
                    std::cerr << "Exception caught\n";
    
                    throw; // rethrow the existing exception
    	}
    };
    ```
    
    - Avoid letting control reach the end of a function-level catch block. Instead, explicitly throw, rethrow, or return.
    - A function-level catch block for a constructor must either throw a new exception or rethrow the existing exception -- they are not allowed to resolve exceptions!
    
    | **Function type** | **Can resolve exceptions via return statement** | **Behavior at end of catch block** |
    | --- | --- | --- |
    | Constructor | No, must throw or rethrow | Implicit rethrow |
    | Destructor | Yes | Implicit rethrow |
    | Non-value returning function | Yes | Resolve exception |
    | Value-returning function | Yes | Undefined behavior |
    - **Function try blocks can catch both base and the current class exceptions**
    - **Don't use function try to clean up resources**
    - If an exception is thrown out of a destructor during stack unwinding, the program will be halted.
- **[[Exception Dangers]]**
    - Exception handling is best used when all of the following are true:
        - The error being handled is likely to occur only infrequently.
        - The error is serious and execution could not continue otherwise.
        - The error cannot be handled at the place where it occurs.
        - There isn't a good alternative way to return an error code back to the caller.
    - Exceptions do come with a small performance price to pay.
    - may also cause it to run slower due to the additional checking that has to be performed.
        - However, the main performance penalty for exceptions happens when an exception is actually thrown.
        - The stack must be unwound, and an appropriate exception handler must be found, which is a relatively expensive operation.
- **[[noexcept]]**
    - **Exception specifications** are a language mechanism that was originally designed to document what kind of exceptions a function might throw as part of a function specification.
    - In C++, all functions are classified as either *non-throwing* or *potentially throwing*. A **non-throwing function** is one that promises not to throw exceptions that are visible to the caller.
        - `void doSomething() noexcept; // this function is specified as non-throwing`
    - A **potentially throwing function** may throw exceptions that are visible to the caller.
    - The `noexcept` specifier has an optional Boolean parameter. `noexcept(true)` is equivalent to `noexcept`, meaning the function is non-throwing. `noexcept(false)` means the function is potentially throwing.
        - Only in template functions
    - Functions that are implicitly non-throwing:
        - Destructors
    - Functions that are non-throwing by default for implicitly-declared or defaulted functions:
        - Constructors: default, copy, move
        - Assignments: copy, move
        - Comparison operators (as of C++20)
    - Functions that are potentially throwing (if not implicitly-declared or defaulted):
        - Normal functions
        - User-defined constructors
        - User-defined operators
    - **The noexcept operator**
        - takes an expression as an argument, and returns `true` or `false` if the compiler thinks it will throw an exception or not
        - This is required to fulfill certain **exception safety guarantees**
    - **Exception safety guarantees**
        - No guarantee -- There are no guarantees about what will happen if an exception is thrown (e.g. a class may be left in an unusable state)
        - Basic guarantee -- If an exception is thrown, no memory will be leaked and the object is still usable, but the program may be left in a modified state.
        - Strong guarantee -- If an exception is thrown, no memory will be leaked and the program state will not be changed. This means the function must either completely succeed or have no side effects if it fails. This is easy if the failure happens before anything is modified in the first place, but can also be achieved by rolling back any changes so the program is returned to the pre-failure state.
        - No throw / No fail guarantee -- The function will always succeed (no-fail) or fail without throwing an exception that is exposed to the caller (no-throw). Exceptions may be thrown internally if not exposed. The `noexcept` specifier maps to this level of exception safety guarantee.
    - **Always make move constructors, move assignment, and swap functions noexcept.**
    - Make copy constructors and copy assignment operators `noexcept` when you can.
    - Use `noexcept` on other functions to express a no-fail or no-throw guarantee.
    - Before C++11, and until C++17, *dynamic exception specifications* were used in place of `noexcept`. The **dynamic exception specifications** syntax uses the `throw` keyword to list which exception types a function might directly or indirectly throw:
- **[[std-move-if-noexcept|std::move_if_noexcept]]**
    - **The problem**
        - Moving an object transfers ownership from source to destination
        - If an exception is thrown mid-move, the source object is left damaged
        - You can't reliably "move back" if the original move already failed
        - This violates the strong exception guarantee
    - **Why copies don't have this problem**
        - Copies don't modify the source, so a failed copy leaves the source intact
        - But copies are slower
    - **`std::move_if_noexcept`**
        - Works like `std::move`, but checks first whether the move constructor is `noexcept`
            - If move constructor is `noexcept` → returns an rvalue, move proceeds
            - If move constructor might throw → returns an lvalue, copy is used instead
        - Gives you the best of both: move when safe, copy when not
    - A `noexcept` move constructor implicitly satisfies the strong exception guarantee
    - Marking your move constructor `noexcept` lets `move_if_noexcept` (and the standard library) use the faster move path
    - `std::vector::resize` and similar functions use `move_if_noexcept` internally
    - Objects with `noexcept` move constructors get faster standard library operations
    - If a type has a throwing move constructor *and* no copy constructor, `move_if_noexcept` will move anyway (no choice), waiving the strong guarantee
