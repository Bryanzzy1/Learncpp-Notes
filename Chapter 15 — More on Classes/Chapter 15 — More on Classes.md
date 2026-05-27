---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/templates
  - cpp/static
  - cpp/friends
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - More on Classes
  - Ch15
up: LearnCPP
related:
  - "[[this Pointer]]"
  - "[[Member Function Chaining]]"
  - "[[Classes and Header Files]]"
  - "[[Nested Types]]"
  - "[[Destructors]]"
  - "[[Class Template Member Functions]]"
  - "[[Static Member Variables]]"
  - "[[Static Member Functions]]"
  - "[[Friend Functions]]"
  - "[[Friend Classes]]"
  - "[[Ref Qualifiers]]"
---

# Chapter 15 — More on Classes

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 26, 2026 9:26 PM

## Notes

- **[[this Pointer]]**
    - Inside every member function, the keyword **this** is a const pointer that holds the address of the current implicit object.
    
    ```cpp
    simple.setID(2);
    Simple::setID(&simple, 2); // note that simple has been changed from an object prefix to a function argument!
    
    void setID(int id) { m_id = id; }
    static void setID(Simple* const this, int id) { this->m_id = id; }
    ```
    
    - The `static` keyword means the function is not associated with objects of the class, but instead is treated as if it were a normal function inside the scope region of the class.
    - **`this` always points to the object being operated on**
- **[[Member Function Chaining]]**
    - **Returning `*this`**
    
    ```cpp
    class Calc
    {
    private:
        int m_value{};
    
    public:
        Calc& add(int value) { m_value += value; return *this; }
        Calc& sub(int value) { m_value -= value; return *this; }
        Calc& mult(int value) { m_value *= value; return *this; }
    
        int getValue() const { return m_value; }
    };
    
    //Allow us to do this:     
    Calc calc{};
    calc.add(5).sub(3).mult(4); // method chaining
    ```
    
    - **Resetting a class back to default state**
    
    ```cpp
    void reset()
    {
        *this = {}; // value initialize a new object and overwrite the implicit object
    }
    ```
    
    - With const member functions, `this` is a const pointer to a const value (meaning the pointer cannot be pointed at something else, nor may the object being pointed to be modified).
- **[[Classes and Header Files]]**
    - Member functions can be defined outside the class definition just like non-member functions. The only difference is that we must prefix the member function names with the name of the class type (in this case, `Date::`) so the compiler knows we're defining a member of that class type rather than a non-member.
    - Define the declarations of a class within a header (.h) file and implement the function within a cpp file.
    - member functions defined outside the class definition can be left in the header file if they are made inline (using the `inline` keyword)
    - Functions defined inside the class definition are implicitly inline, which allows them to be #included into multiple code files without violating the ODR.
    - Put any default arguments for member functions inside the class definition.
- **[[Nested Types]]**
    - To create a nested type, you simply define the type inside the class, under the appropriate access specifier.
    
    ```cpp
    class Fruit
    {
    public:
    	// FruitType has been moved inside the class, under the public access specifier
            // We've also renamed it Type and made it an enum rather than an enum class
    	enum Type
    	{
    		apple,
    		banana,
    		cherry
    	};
    
    private:
    	Type m_type {};
    	int m_percentageEaten { 0 };
    
    public:
    	Fruit(Type type) :
    		m_type { type }
    	{
    	}
    
    	Type getType() { return m_type;  }
    	int getPercentageEaten() { return m_percentageEaten;  }
    
    	bool isCherry() { return m_type == cherry; } // Inside members of Fruit, we no longer need to prefix enumerators with FruitType::
    };
    ```
    
    - Class types can also contain nested typedefs or type aliases
    
    ```cpp
    public:
        using IDType = int;
    ```
    
    - It is fairly uncommon for classes to have other classes as a nested type, but it is possible.
        - a nested class does not have access to the `this` pointer of the outer (containing) class, so nested classes can not directly access the members of the outer class.
        - However, because nested classes are members of the outer class, they can access any private members of the outer class that are in scope.
    - A nested type can be forward declared within the class that encloses it.
        - a nested type cannot be forward declared prior to the definition of the enclosing class.
- **[[Destructors]]**
    - Destructors are designed to allow a class to do any necessary clean up before an object of the class is destroyed.
    - Rules:
        1. The destructor must have the same name as the class, preceded by a tilde (~).
        2. The destructor can not take arguments.
        3. The destructor has no return type.
    - A class can only have a single destructor.
    
    ```cpp
        ~Simple() // here's our destructor
        {
            std::cout << "Destructing Simple " << m_id << '\n';
        }
    ```
    
    - If a non-aggregate class type object has no user-declared destructor, the compiler will generate a destructor with an empty body.
    - When using `std::exit()` , the program just ends, the local variables are not destroyed first, and because of this, no destructors will be called.
        - Unhandled exceptions will also cause the program to terminate, and may not unwind the stack before doing so. If stack unwinding does not happen, destructors will not be called prior to the termination of the program.
- **[[Class Template Member Functions]]**
    
    ```cpp
    template <typename T>
    class Pair
    {
    private:
        T m_first{};
        T m_second{};
    
    public:
        // When we define a member function inside the class definition,
        // the template parameter declaration belonging to the class applies
        Pair(const T& first, const T& second)
            : m_first{ first }
            , m_second{ second }
        {
        }
    
        bool isEqual(const Pair<T>& pair);
    };
    
    // When we define a member function outside the class definition,
    // we need to resupply a template parameter declaration
    template <typename T>
    bool Pair<T>::isEqual(const Pair<T>& pair)
    {
        return m_first == pair.m_first && m_second == pair.m_second;
    }
    ```
    
    - **Injected class names**
        - Within the scope of a class, the unqualified name of the class is called an **injected class name**. In a class template, the injected class name serves as shorthand for the fully templated name.
    - Any member function templates defined outside the class definition should be defined just below the class definition (in the same file).
    - Non-aggregate classes don't need deduction guides. A matching constructor gives the compiler enough info to deduce template parameters automatically.
    - The compiler needs to see both the class definition *and* the function definition together to instantiate the template. Functions from templates are implicitly inline, so including them in multiple files is fine.
- **[[Static Member Variables]]**
    - **static member variables** are shared by all objects of the class
    - **Static members are not associated with class objects**
        - Static members are global variables that live inside the scope region of the class.
    - Because static member `s_value` exists independently of any class objects, it can be accessed directly using the class name and the scope resolution operator
    - Note that this static member definition is not subject to access controls: you can define and initialize the value even if it's declared as private (or protected) in the class (as definitions are not considered to be a form of access).
    - For non-template classes, if the class is defined in a header (.h) file, the static member definition is usually placed in the associated code file for the class (e.g. `Something.cpp`).
    
    ```cpp
    class Whatever
    {
    public:
        static const int s_value{ 4 }; // a static const int can be defined and initialized directly
    };
    ```
    
    - Inline static memeber variables can be initialized inside the class definition regardless of whether they are constant or not. This is the preferred method of defining and initializing static members.
    - **Only static members may use type deduction (`auto` and CTAD)**
- **[[Static Member Functions]]**
    - Because static member functions are not associated with a particular object, they can be called directly by using the class name and the scope resolution operator (e.g. `Something::getValue()`). Like static member variables, they can also be called through objects of the class type, though this is not recommended.
    
    ```cpp
    public:
        static int getValue() { return s_value; } // static member function
    ```
    
    - **Static member functions have no `this` pointer**
    - static member functions can directly access other static members (variables or functions), but not non-static members.
    - Static member functions can also be defined outside of the class declaration. This works the same way as for normal member functions.
    - Instead of writing a class with all static members, consider writing a normal class and instantiating a global instance of it (global variables have static duration). That way the global instance can be used when appropriate, but local instances can still be instantiated if and when that is useful.
    - In general, a static class is preferable when you have static data members and/or need access controls. Otherwise, prefer a namespace.
    - **C++ does not support static constructors**
- **[[Friend Functions]]**
    - a **friend declaration** (using the `friend` keyword) can be used to tell the compiler that some other class or function is now a friend.
        - a **friend** is a class or function (member or non-member) that has been granted full access to the private and protected members of another class.
        - a class can selectively give other classes or functions full access to their members without impacting anything else.
        
        ```cpp
         // Here is the friend declaration that makes non-member function void print(const Accumulator& accumulator) a friend of Accumulator
            friend void print(const Accumulator& accumulator);
            
        void print(const Accumulator& accumulator)
        {
            // Because print() is a friend of Accumulator
            // it can access the private members of Accumulator
            std::cout << accumulator.m_value;
        }
        ```
        
        - Note that because `print()` is a non-member function (and thus does not have an implicit object), we must explicitly pass an `Accumulator` object to `print()` to work with.
        - **Defining a friend non-member inside a class**
            - Friend functions defined inside a class are non-member functions
        - A friend function should prefer to use the class interface over direct access whenever possible.
        - **Prefer non-friend functions to friend functions**
- **[[Friend Classes]]**
    - A **friend class** is a class that can access the private and protected members of another class.
    - Class friendship is also not transitive. If class A is a friend of B, and B is a friend of C, that does not mean A is a friend of C.
        - Nor is friendship inherited. If class A makes B a friend, classes derived from B are not friends of A.
    - **Friend member functions**
        - This is done similarly to making a non-member function a friend, except the name of the member function is used instead.
        
        ```cpp
        // Make the Display::displayStorage member function a friend of the Storage class
        	friend void Display::displayStorage(const Storage& storage); // error: Storage hasn't seen the full definition of class Display
        ```
        
        - Define the display class first, and have a forward declaration if needed, and then you can friend the member function
- **[[Ref Qualifiers]]**
    - C++11 introduced a little known feature called a **ref-qualifier** that allows us to overload a member function based on whether it is being called on an lvalue or an rvalue implicit object.
    - we only want one function to service two different cases (one where our implicit object is an lvalue, and one where our implicit object is an rvalue).
    
    ```cpp
    const std::string& getName() const &  { return m_name; } //  & qualifier overloads function to match only lvalue implicit objects, returns by reference
    std::string        getName() const && { return m_name; } // && qualifier overloads function to match only rvalue implicit objects, returns by value
    ```
    
    - instead of having the rvalue getter return a (possibly expensive) copy of the member, we can have it try to move the member (using `std::move`).
    
    ```cpp
    // If the implicit object is a non-const rvalue, use std::move to try to move m_name
    std::string getName() && { return std::move(m_name); }
    ```
    
    - non-ref-qualified overloads and ref-qualified overloads cannot coexist.
    - if only a const lvalue-qualified function exists, it will accept either lvalue or rvalue implicit objects.
    - either qualified overload can be explicitly deleted (using `= delete`), which prevents calls to that function.
    - not recommending the use of ref-qualifiers as a best practice. Instead, we recommend always using the result of an access function immediately and not saving returned references for use later.
