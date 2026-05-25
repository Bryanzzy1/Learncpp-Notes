---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/constructors
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Introduction to Classes
  - Ch14
up: LearnCPP
related:
  - "[[Object-oriented Programming]]"
  - "[[Classes]]"
  - "[[Member Functions]]"
  - "[[Const Member Functions]]"
  - "[[Member Access]]"
  - "[[Data Hiding]]"
  - "[[Constructors]]"
  - "[[Member Initializer List]]"
  - "[[Default Constructor]]"
  - "[[Delegating Constructors]]"
  - "[[Temporary Class Objects]]"
  - "[[Copy Constructor]]"
  - "[[Copy Elision]]"
  - "[[Converting Constructors]]"
  - "[[Constexpr Classes]]"
---

# Chapter 14 — Introduction to Classes

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 24, 2026 10:03 PM

## Notes

- **[[Object-oriented Programming]]**
    - The focus is on creating program-defined data types that contain both properties and a set of well-defined behaviors.
- **[[Classes]]**
    - A **class invariant** is a condition that must be true throughout the lifetime of an object in order for the object to remain in a valid state.
    
    ```cpp
    class Date       // we changed struct to class
    {
    public:          // and added this line, which is called an access specifier
        int m_day{}; // and added "m_" prefixes to each of the member names
        int m_month{};
        int m_year{};
    };
    ```
    
    - **[[Member Functions]]**
        - Member functions must be declared inside the class type definition, and can be defined inside or outside of the class type definition.
        - The object that a member function is called on is *implicitly* passed to the member function. For this reason, the object that a member function is called on is often called **the implicit object**.
        - **Member variables and functions can be defined in any order**
            - Data members are initialized in order of declaration.
            - When the compiler encounters a member function defined inside the class definition:
                - The member function is implicitly forward declared.
                - The member function definition is moved immediately after the end of the class definition.
                
                ```cpp
                //Complier will compile to some form like this
                struct Foo
                {
                    int z(); // forward declaration of Foo::z()
                    int x(); // forward declaration of Foo::x()
                    int y(); // forward declaration of Foo::y()
                
                    int m_data{};
                };
                
                int Foo::z() { return m_data; } // m_data already declared above
                int Foo::x() { return y(); }    // y already declared above
                int Foo::y() { return 5; }
                ```
                
        - **Member functions can be overloaded**
        - Member functions can be used with both structs and classes.
            - However, structs should avoid defining constructor member functions, as doing so makes them a non-aggregate.
    - If your class type has no data members, prefer using a namespace.
    - **[[Const Member Functions]]**
        - **Modifying the data members of const objects is disallowed**
            - **Const objects may not call non-const member functions**
            
            ```cpp
                void print() const // now a const member function
                {
                    std::cout << year << '/' << month << '/' << day;
                }
            ```
            
            - A const member function may not: modify the implicit object, call non-const member functions.
            - A const member function may: modify objects that aren't the implicit object, call const member functions, call non-member functions.
        - A member function that does not (and will not ever) modify the state of the object should be made const, so that it can be called on both const and non-const objects.
    - **[[Member Access]]**
        - **The members of a struct are public by default**
            - **Public members** are members of a class type that do not have any restrictions on how they can be accessed.
        - **The members of a class are private by default**
            - **Private members** are members of a class type that can only be accessed by other members of the same class.
            - A class with private members is no longer an aggregate, and therefore can no longer use aggregate initialization.
            - In C++, it is a common convention to name private data members starting with an "m_" prefix.
        - **Setting access levels via access specifiers**
            
            ```cpp
            class Date
            {
            // Any members defined here would default to private
            
            public: // here's our public access specifier
            
                void print() const // public due to above public: specifier
                {
                    // members can access other private members
                    std::cout << m_year << '/' << m_month << '/' << m_day;
                }
            
            private: // here's our private access specifier
            
                int m_year { 2020 };  // private due to above private: specifier
                int m_month { 14 }; // private due to above private: specifier
                int m_day { 10 };   // private due to above private: specifier
            };
            ```
            
            - Protected allows derived class access whereas private does not
        - IMPORTANT: Access levels are per-class, not per-object, a member function can also directly access the private members of ANY other object of the same class type that is in scope.
        - The ONLY difference between class and struct is that: A class defaults its members to private, whereas a struct defaults its members to public.
            - Minor: structs inherit from other class types publicly and classes inherit privately.
        - **Access functions**
            - A trivial public member function whose job is to retrieve or change the value of a private member variable.
            - Getters/accessors or Setters/mutators
                - **Getters should return by value or by const lvalue reference**
                - Data members have the same lifetime as the object containing them.
            - Prefer to use the return value of a member function that returns by reference immediately, to avoid issues with dangling references when the implicit object is an rvalue.
            - **Do not return non-const references to private data members**
- **[[Data Hiding]]**
    - The **interface** of a class type (also called a **class interface**) defines how a user of the class type will interact with objects of the class type.
        - an interface composed of public members is sometimes called a **public interface**.
    - **Data hiding** (also called **information hiding** or **data abstraction**) is a technique used to enforce the separation of interface and implementation by hiding (making inaccessible) the implementation of a program-defined data type from users.
    - The term **encapsulation** typically refers to one of two things:
        - The enclosing of one or more items within a container of some kind.
        - The bundling of data and functions for operating on instances of that data.
    - To use an encapsulated class, you don't need to know how it is implemented. You only need to understand its interface: what member functions are publicly available, what arguments they take, and what values they return.
    - Declare public members first, protected members next, and private members last. This spotlights the public interface and de-emphasizes implementation details.
- **[[Constructors]]**
    - A **constructor** is a special member function that is automatically called after a non-aggregate class type object is created.
    - When a non-aggregate class type object is defined, the compiler looks to see if it can find an accessible constructor that is a match for the initialization values provided by the caller (if any).
        - If an accessible matching constructor is found, memory for the object is allocated, and then the constructor function is called.
        - If no accessible matching constructor can be found, a compilation error will be generated.
    - Constructors must have the same name as the class (with the same capitalization). For template classes, this name excludes the template parameters.
    - Constructors have no return type (not even `void`).
    - **Constructor implicit conversion of arguments**
    - **Constructors should not be const**
    - **[[Member Initializer List]]**
        
        ```cpp
            Foo(int x, int y)
                : m_x { x }, m_y { y } // here's our member initialization list
            {
                std::cout << "Foo(" << x << ", " << y << ") constructed\n";
            }
        ```
        
        - Member variables in a member initializer list should be listed in order that they are defined in the class.
        - This means that if a member has both a default member initializer and is listed in the member initializer list for the constructor, the member initializer list value takes precedence.
        - The bodies of constructors functions are most often left empty.
    - **[[Default Constructor]]** is a constructor that accepts no arguments. Typically, this is a constructor that has been defined with no parameters.
        - Prefer value initialization over default initialization for all class types.
        
        ```cpp
            Foo(int x=0, int y=0) // has default arguments
                : m_x { x }
                , m_y { y }
            {
                std::cout << "Foo(" << m_x << ", " << m_y << ") constructed\n";
            }
        ```
        
        - You can also have **Overloaded constructors**
        - If a non-aggregate class type object has no user-declared constructors, the compiler will generate a public default constructor (so that the class can be value or default initialized). This constructor is called an **implicit default constructor**.
        - **Using `= default` to generate an explicitly defaulted default constructor**
            - **explicitly defaulted default constructor**
            
            ```cpp
            Foo() = default; // generates an explicitly defaulted default constructor
            ```
            
            - Prefer an explicitly defaulted default constructor (`= default`) over a default constructor with an empty body.
        - When value initializing a class, if the class has a user-defined default constructor, the object will be default initialized, if the class has a default constructor that is not user-provided (that is, a default constructor that is either implicitly defined, or defined using `= default`), the object will be zero-initialized before being default initialized.
        - Prior to C++20, a class with a user-defined default constructor (even if it has an empty body) makes the class a non-aggregate, whereas an explicitly defaulted default constructor does not.
    - **[[Delegating Constructors]]**
        - Constructors should not be called directly from the body of another function. Doing so will either result in a compilation error, or will direct-initialize a temporary object.
        - If you do want a temporary object, prefer list-initialization (which makes it clear you are intending to create an object).
        - If you have multiple constructors, consider whether you can use delegating constructors to reduce duplicate code.
- **[[Temporary Class Objects]]**
    - A **temporary object** (sometimes called an **anonymous object** or an **unnamed object**) is an object that has no name and exists only for the duration of a single expression.
    
    ```cpp
    void print(IntPair p)
    {
        std::cout << "(" << p.x() << ", " << p.y() << ")\n";
    }
    
    int main()
    {
        // Case 1: Pass variable
        IntPair p { 3, 4 };
        print(p);
    
        // Case 2: Construct temporary IntPair and pass to function
        print(IntPair { 5, 6 } );
    
        // Case 3: Implicitly convert { 7, 8 } to a temporary Intpair and pass to function
        print( { 7, 8 } );
    ```
    
    - There is no syntax to create temporary objects using copy initialization.
    - When a function returns by value, the object that is returned is a temporary object (initialized using the value or object identified in the return statement).
    - Prefer `static_cast` when converting to a fundamental type, and a list-initialized temporary when converting to a class type.
        - Prefer `static_cast` when to create a temporary object when any of the following are true:
            - We need to performing a narrowing conversion.
            - We want to make it really obvious that we're converting to a type that will result in some different behavior (e.g. a `char` to an `int`).
            - We want to use direct-initialization for some reason (e.g. to avoid list constructors taking precedence).
        - Prefer creating a new object (using list initialization) to create a temporary object when any of the following are true:
            - We want to use list-initialization (e.g. for the protection against narrowing conversions, or because we need to invoke a list constructor).
            - We need to provide additional arguments to a constructor to facilitate the conversion.
- **[[Copy Constructor]]**
    - A **copy constructor** is a constructor that is used to initialize an object with an existing object of the same type.
    - After the copy constructor executes, the newly created object should be a copy of the object passed in as the initializer.
    - If you do not provide a copy constructor for your classes, C++ will create a public **implicit copy constructor** for you.
        - The implicit copy constructor will do memberwise initialization.
    
    ```cpp
        // Copy constructor
        Fraction(const Fraction& fraction)
            // Initialize our members using the corresponding member of the parameter
            : m_numerator{ fraction.m_numerator }
            , m_denominator{ fraction.m_denominator }
        {
            std::cout << "Copy constructor called\n"; // just to prove it works
        }
    ```
    
    - Copy constructors should have no side effects beyond copying.
    - Prefer the implicit copy constructor, unless you have a specific reason to create your own.
    - **If you write your own copy constructor, the parameter should be a const lvalue reference.**
        - When an object is passed by value, the argument is copied into the parameter. When the argument and parameter are the same class type, the copy is made by implicitly invoking the copy constructor.
    - **Using `= default` to generate a default copy constructor**
    
    ```cpp
        // Explicitly request default copy constructor
        Fraction(const Fraction& fraction) = default;
    ```
    
    - **Using `= delete` to prevent copies**
        - do not want objects of a certain class to be copyable.
        
        ```cpp
            // Delete the copy constructor so no copies can be made
            Fraction(const Fraction& fraction) = delete;
        ```
        
    - The **rule of three** is a well known C++ principle that states that if a class requires a user-defined copy constructor, destructor, or copy assignment operator, then it probably requires all three.
        - In C++11, this was expanded to the **rule of five**, which adds the move constructor and move assignment operator to the list.
- **[[Copy Elision]]**
    - There are three key differences between the initialization forms:
        - List initialization disallows narrowing conversions.
        - Copy initialization only considers non-explicit constructors/conversion functions.
        - List initialization prioritizes matching list constructors over other matching constructors.
    - **Copy elision** is a compiler optimization technique that allows the compiler to remove unnecessary copying of objects.
        - When the compiler optimizes away a call to the copy constructor, we say the constructor has been **elided**.
    - **Copy elision in pass by value and return by value**
    - **Mandatory copy elision in C++17**
- **[[Converting Constructors]]**
    - **Only one user-defined conversion may be applied**
    - A constructor that can be used to perform an implicit conversion is called a **converting constructor**.
        - By default, all constructors are converting constructors.
    - **The explicit keyword**
        - use the **explicit** keyword to tell the compiler that a constructor should not be used as a converting constructor.
        - An explicit constructor cannot be used to do copy initialization or copy list initialization.
        - An explicit constructor cannot be used to do implicit conversions (since this uses copy initialization or copy list initialization).
    
    ```cpp
        explicit Dollars(int d) // now explicit
            : m_dollars{ d }
        {
        }
    ```
    
    - When we return a value from a function, if that value does not match the return type of the function, an implicit conversion will occur. Just like with pass by value, such conversions cannot use explicit constructors.
    - Copy (and move) constructors (as these do not perform conversions) should not be made explicit
    - Default constructors with no parameters (as these are only used to convert `{}` to a default object, not something we typically need to restrict) should typically not be made explicit
    - Constructors that only accept multiple arguments (as these are typically not a candidate for conversions anyway) should typically not be made explicit
    - Constructors that take a single argument should typically be made explicit
- **[[Constexpr Classes]]**
    - In C++, a **literal type** is any type for which it might be possible to create an object within a constant expression.
    - aggregates implicitly support constexpr
    - If you want your class to be able to be evaluated at compile-time, make your member functions and constructor constexpr.
    - When a constexpr function is evaluating in a compile-time context, only constexpr functions can be called.
    - As of C++14, constexpr member functions are no longer implicitly const. This means that if you want a constexpr function to be a const function, you must explicitly mark it as such.
    - **Constexpr non-const member functions can change data members**
    - `constexpr const int& getX() const { return m_x; }`
        - The `constexpr` indicates that the member function can be evaluated at compile-time. The `const int&` is the return type of the function. The rightmost `const` means the member-function itself is const so it can be called on const objects.
