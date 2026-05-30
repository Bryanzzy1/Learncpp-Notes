# Chapter 16 — Arrays, Strings, Pointers

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 30, 2026 3:15 PM

## Notes

- **Containers and arrays**
    - a **container** is a data type that provides storage for a collection of unnamed objects (called **elements**).
    - We typically use containers when we need to work with a set of related values.
    - We’ll prefer the term “length” when referring to the number of elements in a container, and use the term “size” to refer to amount of storage required by an object.
    - In most programming languages (including C++), containers are **homogenous**, meaning the elements of a container are required to have the same type.
    - A class type that implements a container is sometimes called a **container class**.
        - C-style arrays
        - `std::string`
        - `std::vector<bool>`
    - **Arrays**
        - An **array** is a container data type that stores a sequence of values **contiguously** (meaning each element is placed in an adjacent memory location, with no gaps).
        - C++ contains three primary array types: (C-style) arrays, the `std::vector` container class, and the `std::array` container class.
- **std::vectors**
    
    ```cpp
    	// Value initialization (uses default constructor)
    	std::vector<int> empty{}; // vector containing 0 int elements
    	
    // List construction (uses list constructor)
    	std::vector<int> primes{ 2, 3, 5, 7 };          // vector containing 4 int elements with values 2, 3, 5, and 7
    	std::vector vowels { 'a', 'e', 'i', 'o', 'u' }; // vector containing 5 char elements with values 'a', 'e', 'i', 'o', and 'u'.  Uses CTAD (C++17) to deduce element type char (preferred).
    ```
    
    - Use list initialization with an initializer list of values to construct a container with those element values.
        - Ensures the container has enough storage to hold all the initialization values (if needed).
        - Sets the length of the container to the number of elements in the initializer list (if needed).
        - Initializes the elements to the values in the initializer list (in sequential order).
    - In C++, the most common way to access array elements is by using the name of the array along with the subscript operator (`operator[]`).
        - This integral value is called a **subscript** (or informally, an **index**).
    - **Arrays are contiguous in memory**
        - Arrays are one of the few container types that allow for **random access**, meaning any element in the container can be accessed directly (as opposed to sequential access, where elements must be accessed in a particular order).
    - `std::vector` has an explicit constructor (`explicit std::vector<T>(std::size_t)`) that takes a single `std::size_t` value defining the length of the `std::vector` to construct
    
    ```cpp
    // vector containing 10 int elements, value-initialized to 0
    std::vector<int> data( 10 ); 
    ```
    
    - When an initializer list is non-empty, a matching list constructor will be selected over other matching constructors.
    
    ```cpp
    // Copy init
    std::vector<int> v1 = 10;     // 10 not an initializer list, copy init won't match explicit constructor: compilation error
    
    // Direct init
    std::vector<int> v2(10);      // 10 not an initializer list, matches explicit single-argument constructor
    
    // List init
    std::vector<int> v3{ 10 };    // { 10 } interpreted as initializer list, matches list constructor
    
    // Copy list init
    std::vector<int> v4 = { 10 }; // { 10 } interpreted as initializer list, matches list constructor
    std::vector<int> v5({ 10 });  // { 10 } interpreted as initializer list, matches list constructor
    
            // Default init
            std::vector<int> v6 {};       // {} is empty initializer list, matches default constructor
            std::vector<int> v7 = {};     // {} is empty initializer list, matches default constructor
    ```
    
    - When constructing a container (or any type that has a list constructor) with initializers that are not element values, use direct initialization.
    - For members of a class type:
    
    ```cpp
    struct Foo
    {
        std::vector<int> v{ std::vector<int>(8) }; // ok
    };
    ```
    
    - A `const std::vector` must be initialized, and then cannot be modified. The elements of such a vector are treated as if they were const.
    - The element type of a `std::vector` must not be defined as const (e.g. `std::vector<const int>` is disallowed).
        - One of the biggest downsides of `std::vector` is that it cannot be made `constexpr`. If you need a `constexpr` array, use `std::array`.
    - **The unsigned length and subscript problem**
        - **The length and indices of `std::vector` have type `size_type`**
            - `size_type` is almost always an alias for `std::size_t`, but can be overridden (in rare cases) to use a different type.
        - `std::size_t` is a typedef for some large unsigned integral type, usually `unsigned long` or `unsigned long long`.
        - `std::vector` (and most other container types in C++) only has `size()`.
        - In C++17, we can also use the `std::size()` non-member function (which for container classes just calls the `size()` member function).
            - Because `std::size()` can also be used on non-decayed C-style arrays, this method is sometimes favored over the using the `size()` member function
        - C++20 introduces the `std::ssize()` non-member function, which returns the length as a large *signed* integral type (usually `std::ptrdiff_t`, which is the type normally used as the signed counterpart to `std::size_t`)
            - Can use `auto` to have the compiler deduce the correct signed type to use for the variable
        - **Accessing array elements using `operator[]` does no bounds checking**
        - **Accessing array elements using the `at()` member function does runtime bounds checking**
            - When the `at()` member function encounters an out-of-bounds index, it actually throws an exception of type `std::out_of_range`.
            - Because it does runtime bounds checking on every call, `at()` is slower (but safer) than `operator[]`.
        - When indexing a `std::vector` with a constexpr (signed) int, we can let the compiler implicitly convert this to a `std::size_t` without it being a narrowing conversion
        - Another good alternative is instead of indexing the `std::vector` itself, index the result of the `data()` member function
            
            ```cpp
            int index { 3 };                          // non-constexpr signed value
            std::cout << prime.data()[index] << '\n'; // okay: no sign conversion warnings
            ```
            
    - **Passing std::vector**
        - Therefore, we typically pass `std::vector` by (const) reference to avoid such copies.
        - Because our `passByRef()` function expects a `std::vector<int>`, we are unable to pass vectors with different element types
        
        ```cpp
        // function template that uses the same template parameter declaration
        template <typename T>
        //void passByRef(const T& arr) // will accept any type of object that has an overloaded operator[]
        //In C++20, we can use an abbreviated function template (via an auto parameter) to do the same thing: void passByRef(const auto& arr)
        void passByRef(const std::vector<T>& arr)
        {
            std::cout << arr[0] << '\n';
        }
        ```
        
        - **Asserting on array length**
            - A better option is to avoid using `std::vector`in cases where you need to assert on array length.
            - Using a type that supports `constexpr` arrays (e.g. `std::array`) is probably a better choice, as you can `static_assert` on the length of a constexpr array.
            - The best option is to avoid writing functions that rely on the user passing in a vector with a minimum length in the first place.
    - **Returning std::vectors**
        - **Copy semantics**
            - Refers to the rules that determine how copies of objects are made.
        - **move semantics**
            - When ownership of data is transferred from one object to another, we say that data has been **moved**.
                - The cost of such a move is typically trivial (usually just two or three pointer assignments, which is way faster than copying an array of data!).
            - the rules that determine how the data from one object is moved to another object.
            - Move semantics are invoked when:
                - The type of the object supports move semantics.
                - The object is being initialized with (or assigned) an rvalue (temporary) object of the same type.
                - The move isn’t elided.
            - `std::vector` and `std::string` both support move semantics
            - **We can return move-capable types like `std::vector` by value**
    - A `std::vector` can be resized after instantiation by calling the `resize()` member function with the new desired length:
    - **capacity** is how many elements the `std::vector` has allocated storage for, and **length** is how many elements are currently being used.
        - We can ask a `std::vector` for its capacity via the `capacity()` member function.
    - **Reallocation of storage**
        - When a `std::vector` changes the amount of storage it is managing, this process is called **reallocation**.
        - **Vector indexing is based on length, not capacity**
    - `std::vector` has a member function called `shrink_to_fit()` that requests that the vector shrink its capacity to match its length.
        - This request is non-binding, meaning the implementation is free to ignore it.
- **Arrays and loops**
    - Any name that depends on a type containing a template parameter is called a **dependent name**. Dependent names must be prefixed with the keyword `typename` in order to be used as a type.
    - If you are dealing with very large arrays, or if you want to be a bit more defensive, you can use the strangely named `std::ptrdiff_t`. This typedef is often used as the signed counterpart to `std::size_t`.
        - Because `std::ptrdiff_t` has a weird name, another good approach is to define your own type alias for indices.
        
        ```cpp
        using Index = std::ptrdiff_t;
        
        // Sample loop using index
        for (Index index{ 0 }; index < static_cast<Index>(arr.size()); ++index)
        ```
        
    - `std::ssize()` in C++20 returns the size of an array type as a signed type (likely `ptrdiff_t`).
    - Avoid array indexing with integral values whenever possible.
    - **Range-based for loops (for-each)**
        
        ```
        for (element_declaration : array_object)
           statement;
        ```
        
        - **Range-based for loops and type deduction using the `auto` keyword**
        
        ```cpp
        std::vector fibonacci { 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89 };
        
        for (auto num : fibonacci) // compiler will deduce type of num to be `int`
           std::cout << num << ' ';
        ```
        
        - Use type deduction (`auto`) with range-based for loops to have the compiler deduce the type of the array element.
        - **Avoid element copies using references**
            - For range-based for loops, prefer to define the element type as:
                - `auto` when you want to modify copies of the elements.
                - `auto&` when you want to modify the original elements.
                - `const auto&` otherwise (when you just need to view the original elements).
        - Range-based for loops work with a wide variety of array types, including (non-decayed) C-style arrays, `std::array`, `std::vector`, linked list, trees, and maps.
        - Range-based for loops won’t work with decayed C-style arrays.
        - **Range-based for loops in reverse** C++20
            - std::views::reverse
            
            ```cpp
            // create a reverse view
            for (const auto& word : std::views::reverse(words)) 
            ```
            
    - **Using unscoped enumerators for indexing**
        
        ```cpp
        #include <vector>
        
        namespace Students
        {
            enum Names
            {
                kenny, // 0
                kyle, // 1
                stan, // 2
                butters, // 3
                cartman, // 4
                max_students // 5
            };
        }
        
        int main()
        {
            std::vector testScores { 78, 94, 66, 77, 14 };
        
            testScores[Students::stan] = 76; // we are now updating the test score belonging to stan
        
            return 0;
        }
        ```
        
        - Since unscoped enumerations will implicitly convert to a `std::size_t`, this means we can use unscoped enumerations as array indices to help document the meaning of the index:
    - **Using a non-constexpr unscoped enumeration for indexing**
    - **Using a count enumerator**
        - its value represents the count of previously defined enumerators.
        - **Asserting on array length with a count enumerator**
        - Use a static_assert to ensure the length of your constexpr array matches your count enumerator.
        - Use an assert to ensure the length of your non-constexpr array matches your count enumerator.
- **Stack behavior with `std::vector`**
    
    
    | **Function Name** | **Stack Operation** | **Behavior** | **Notes** |
    | --- | --- | --- | --- |
    | push_back() | Push | Put new element on top of stack | Adds the element to end of vector |
    | pop_back() | Pop | Remove the top element from the stack | Returns void, removes element at end of vector |
    | back() | Top or Peek | Get the top element on the stack | Does not remove item |
    | emplace_back() | Push | Alternate form of push_back() that can be more efficient (see below) | Adds element to end of vector |
    - `push_back()` and `emplace_back()` will increment the length of a `std::vector`, and will cause a reallocation to occur if the capacity is not sufficient to insert the value.
        - in cases where we are creating a temporary object (of the same type as the vector’s element) for the purpose of pushing it onto the vector, `emplace_back()` can be more efficient
    - The `resize()` member function changes the length of the vector, and the capacity (if necessary).
    - **The `reserve()` member function changes the capacity (but not the length)**
- **std::vector<bool>**
    - Favor `constexpr std::bitset`, `std::vector<char>`, or 3rd party dynamic bitsets over `std::vector<bool>`.