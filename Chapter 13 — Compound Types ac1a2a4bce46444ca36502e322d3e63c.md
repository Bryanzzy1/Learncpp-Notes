# Chapter 13 — Compound Types

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 18, 2026 2:55 PM

## Notes

- **Program-defined (user-defined) types**
    
    ```cpp
    struct Fraction
    {
    	int numerator {};
    	int denominator {};
    };
    ```
    
    - **Type definitions are partially exempt from the one-definition rule (ODR)**
- **Enumerations**
    - **Unscoped enumerations**
        - An **enumeration** (also called an **enumerated type** or an **enum**) is a compound data type whose values are restricted to a set of named symbolic constants (called **enumerators**).
        
        ```cpp
        // Define a new unscoped enumeration named Color
        // this enum is defined in the global namespace
        enum Color
        {
            // Here are the enumerators
            // These symbolic constants define all the possible values this type can hold
            // Each enumerator is separated by a comma, not a semicolon
            red, // so red is put into the global namespace. 0
            green, // 1
            blue, // trailing comma optional but recommended, 2
        }; // the enum definition must end with a semicolon
        ```
        
        - Each enumerated type you create is considered to be a **distinct type**, meaning the compiler can distinguish it from other types (unlike typedefs or type aliases, which are considered non-distinct from the types they are aliasing).
        - **Avoiding enumerator naming collisions**
            - Prefer putting your enumerations inside a named scope region (such as a namespace or class) so the enumerators don’t pollute the global namespace.
        - Avoid assigning explicit values to your enumerators unless you have a compelling reason to do so.
        - When an enumerated type is used in a function call or with an operator, the compiler will first try to find a function or operator that matches the enumerated type.
        - Enumerators have values that are of an integral type. The specific integral type used to represent the value of enumerators is called the enumeration’s **underlying type** (or **base**).
            - `enum Color : std::int8_t`
            - Specify the base type of an enumeration only when necessary.
        - It is also safe to static_cast any integral value that is in range of the target enumeration’s underlying type, even if there are no enumerators representing that value.
        - `Pet pet { static_cast<Pet>(2) }; // convert integer 2 to a Pet`
            
            ```cpp
            enum Pet: int // we've specified a base
            {
                cat, // assigned 0
                dog, // assigned 1
                pig, // assigned 2
                whale, // assigned 3
            };
            
            int main()
            {
                Pet pet1 { 2 }; // ok: can brace initialize unscoped enumeration with specified base with integer (C++17)
                Pet pet2 (2);   // compile error: cannot direct initialize with integer
                Pet pet3 = 2;   // compile error: cannot copy initialize with integer
            
                pet1 = 3;       // compile error: cannot assign with integer
            
                return 0;
            }
            ```
            
    - **Scoped Enumeration**
        - They won’t implicitly convert to integers, and the enumerators are *only* placed into the scope region of the enumeration (not into the scope region where the enumeration is defined).
        
        ```cpp
        int main()
        {
            enum class Color // "enum class" defines this as a scoped enumeration rather than an unscoped enumeration
            {
                red, // red is considered part of Color's scope region
                blue,
            };
        
            enum class Fruit
            {
                banana, // banana is considered part of Fruit's scope region
                apple,
            };
        
            Color color { Color::red }; // note: red is not directly accessible, we have to use Color::red
            Fruit fruit { Fruit::banana }; // note: banana is not directly accessible, we have to use Fruit::banana
        
            if (color == fruit) // compile error: the compiler doesn't know how to compare different types Color and Fruit
                std::cout << "color and fruit are equal\n";
            else
                std::cout << "color and fruit are not equal\n";
        
            return 0;
        }
        ```
        
        - In C++23, use `std::to_underlying()` (defined in the <utility> header), which converts an enumerator to a value of the underlying type of the enumeration.
        - Favor scoped enumerations over unscoped enumerations unless there’s a compelling reason to do otherwise.
        - One thing to speed up conversion is to overload the unary operator + to perform this conversion
        
        ```cpp
        // Overload the unary + operator to convert an enum to the underlying type
        // adapted from https://stackoverflow.com/a/42198760, thanks to Pixelchemist for the idea
        // In C++23, you can #include <utility> and return std::to_underlying(a) instead
        template <typename T>
        constexpr auto operator+(T a) noexcept
        {
            return static_cast<std::underlying_type_t<T>>(a);
        }
        
        int main()
        {
            std::cout << +Animals::elephant << '\n'; // convert Animals::elephant to an integer using unary operator+
        
            return 0;
        }
        ```
        
        - Introduced in C++20, a `using enum` statement imports all of the enumerators from an enum into the current scope.
- **Overloading the I/O operators**
    - Steps:
        - Define a function using the name of the operator as the function’s name.
        - Add a parameter of the appropriate type for each operand (in left-to-right order). One of these parameters must be a user-defined type (a class type or an enumerated type), otherwise the compiler will error.
        - Set the return type to whatever type makes sense.
        - Use a return statement to return the result of the operation.
    - When this expression is evaluated, the compiler will look for an overloaded `operator<<` function that can handle arguments of type `std::ostream` and `int`.
        
        ```cpp
        // Teach operator<< how to print a Color
        // std::ostream is the type of std::cout, std::cerr, etc...
        // The return type and parameter type are references (to prevent copies from being made)
        std::ostream& operator<<(std::ostream& out, Color color)
        {
            out << getColorName(color); // print our color's name to whatever output stream out
            return out;                 // operator<< conventionally returns its left operand
        
            // The above can be condensed to the following single line:
            // return out << getColorName(color)
        }
        
        int main()
        {
        	Color shirt{ blue };
        	std::cout << "Your shirt is " << shirt << '\n'; // it works!
        
        	return 0;
        }
        ```
        
- **Structs, members, and member selection**
    - A struct is a class type (as are classes and unions). As such, anything that applies to class types applies to structs.
    
    ```cpp
    struct Employee
    {
        int id {};
        int age {};
        double wage {};
    };
    
    int main()
    {
        Employee joe {};
    
        joe.age = 32;  // use member selection operator (.) to select the age member of variable joe
    
        std::cout << joe.age << '\n'; // print joe's age
    
        return 0;
    }
    ```
    
    - The variables that are part of the struct are called **data members** (or **member variables**).
    - **Data members are not initialized by default**
    - An **aggregate data type** (also called an **aggregate**) is any type that can contain multiple data members.
        - Aggregates use a form of initialization called **aggregate initialization**, which allows us to directly initialize the members of aggregates. To do this, we provide an **initializer list** as an initializer, which is just a braced list of comma-separated values.
        - **memberwise initialization**, which means each member in the struct is initialized in the order of declaration
        - Prefer the (non-copy) braced list form when initializing aggregates.
            - `Employee joe { 2, 28, 45000.0 };`
    - Variables of a struct type can be const (or constexpr), and just like all const variables, they must be initialized.
    - C++20 adds a new way to initialize struct members called **designated initializers**.
        - `Foo f1{ .a{ 1 }, .c{ 3 } };` , `Foo f2{ .a = 1, .c = 3 };`
    - **Initializing a struct with another struct of the same type**
    
    ```cpp
    Foo foo { 1, 2, 3 };
    
        Foo x = foo; // copy-initialization
        Foo y(foo);  // direct-initialization
        Foo z {foo}; // direct-list-initialization
    ```
    
    - **Default member initializer**
        - **Explicit initialization values take precedence over default values**
        - **Always provide default values for your members**
    
    ```cpp
    struct Something
    {
        int x;       // no initialization value (bad)
        int y {};    // value-initialized by default
        int z { 2 }; // explicit default value
    };
    ```
    
    - **Passing structs (by reference)**
    
    ```cpp
    void printEmployee(const Employee& employee) // note pass by reference here
    {
        std::cout << "ID:   " << employee.id << '\n';
        std::cout << "Age:  " << employee.age << '\n';
        std::cout << "Wage: " << employee.wage << '\n';
    }
    ```
    
    - **Passing temporary structs**
        
        ```cpp
        printEmployee(Employee { 14, 32, 24.15 }); // construct a temporary Employee to pass to function (type explicitly specified) (preferred)
        ```
        
    - Returning a struct
    
    ```cpp
    Point3d getZeroPoint()
    {
        // We already specified the type at the function declaration
        // so we don't need to do so here again
        return { 0.0, 0.0, 0.0 }; // return an unnamed Point3d
    }
    
    return {}; //this works too!
    ```
    
    - In most cases, we want our structs (and classes) to be owners. The easiest way to enable this is to ensure each data member has an owning type (e.g. not a viewer, pointer, or reference).
    - We can only say that the size of a struct will be *at least* as large as the size of all the variables it contains.
        - For performance reasons, the compiler will sometimes add gaps into structures (this is called **padding**).
        - data structure alignment
    - **Member selection for pointers to structs**
        
        ```cpp
        Employee* ptr{ &joe };
        
        std::cout << ptr->id << '\n'; // Better: use -> to select member from pointer to object
        ```
        
        - You can chain**`operator->`**
- **Class templates**
    - a template definition for instantiating class types.
    - A “class type” is a struct, class, or union type.
    
    ```cpp
    template <typename T>
    struct Pair
    {
        T first{};
        T second{};
    };
    ```
    
    - **Using class template in a function**
    
    ```cpp
    template <typename T>
    struct Pair
    {
        T first{};
        T second{};
    };
    
    template <typename T>
    constexpr T max(Pair<T> p)
    {
        return (p.first < p.second ? p.second : p.first);
    }
    ```
    
    - Class templates can have some members using a template type and other members using a normal (non-template) type.
    
    ```cpp
    template <typename T>
    struct Foo
    {
        T first{};    // first will have whatever type T is replaced with
        int second{}; // second will always have type int, regardless of what type T is
    };
    ```
    
    - Class templates can also have multiple template types.
    - **std::pair**
        - The C++ standard library contains a class template named `std::pair` (in the `<utility>` header) that is defined identically to the `Pair` class template with multiple template types in the preceding section.
    - **Function Templates with Multiple Types**
        
        ```cpp
        void print(Pair<T, U> p) // only accepts Pair<T, U>
        
        template <typename T>
        void print(T p) // matches ANY type that has .first and .second
        
        template <typename Pair> // ⚠️ this shadows the Pair struct!
        void print(Pair p)       // "Pair" here means the template param, not the class
        ```
        
- **Class template argument deduction (CTAD) and deduction guides**
    - **Class template argument deduction (CTAD)**
        - Starting in C++17, when instantiating an object from a class template, the compiler can deduce the template types from the types of the object’s initializer
        - CTAD is only performed if no template argument list is present.
        
        ```cpp
            std::pair p1 { 3.4f, 5.6f }; // deduced to pair<float, float>
            std::pair p2 { 1u, 2u };     // deduced to pair<unsigned int, unsigned int>
        ```
        
        - When initializing the member of a class type using non-static member initialization, CTAD will not work in this context. All template arguments must be explicitly specified
        - **CTAD doesn’t work with function parameters**
        - **CTAD doesn’t work with non-static member initialization**
    - **Template argument deduction guides**
        - tells the compiler how to deduce the template arguments for a given class template.
        
        ```cpp
        // Here's a deduction guide for our Pair (needed in C++17 only)
        // Pair objects initialized with arguments of type T and U should deduce to Pair<T, U>
        template <typename T, typename U>
        Pair(T, U) -> Pair<T, U>;
        ```
        
        - So when the compiler sees the definition `Pair p2{ 1, 2 };` in our program, it will say, “oh, this is a declaration of a `Pair` and there are two arguments of type `int` and `int`, so using the deduction guide, I should deduce this to be a `Pair<int, int>`“.
        - C++20 added the ability for the compiler to automatically generate deduction guides for aggregates, so deduction guides should only need to be provided for C++17 compatibility.
        - Non-aggregates don’t need deduction guides in C++17 because the presence of a constructor serves the same purpose.
    - **Type template parameters with default values**
    
    ```cpp
    template <typename T=int, typename U=int> // default T and U to type int
    ```
    
- **Alias templates**
    - Creating a type alias for a class template where all template arguments are explicitly specified works just like a normal type alias
        
        ```cpp
        using Point = Pair<int>; // create normal type alias
        Point p { 1, 2 };        // compiler replaces this with Pair<int>
        ```
        
    - a template that can be used to instantiate type aliases.
        - the user still supplies the type argument
        
        ```cpp
        // Alias templates must be defined in global scope
        template <typename T>
        using Coord = Pair<T>; // Coord is an alias for Pair<T>
        ```
        
    - In C++20, we can use alias template deduction to deduce the template arguments in cases where CTAD works