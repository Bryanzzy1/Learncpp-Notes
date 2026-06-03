# Chapter 17 — Dynamic memory allocation

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 3, 2026 6:09 PM

## Notes

- **std::array**
    - Use `std::array` for constexpr arrays, and `std::vector` for non-constexpr arrays.
    
    ```cpp
        std::array<int, 5> a {};  // a std::array of 5 ints
    
        std::vector<int> b(5);    // a std::vector of 5 ints (for comparison)
    ```
    
    - **The length of a `std::array` must be a constant expression**
    - a `std::array` can be defined with a length of 0
        - A zero-length `std::array` is a special-case class that has no data.
    - **Aggregate initialization of a `std::array`**
        - If a `std::array` is defined without an initializer, the elements will be default initialized. In most cases, this will result in elements being left uninitialized.
        - If more initializers are provided in an initializer list than the defined array length, the compiler will error. If fewer initializers are provided in an initializer list than the defined array length, the remaining elements without initializers are value initialized
    - Define your `std::array` as constexpr whenever possible. If your `std::array` is not constexpr, consider using a `std::vector` instead.
    - Use class template argument deduction (CTAD) to have the compiler deduce the type and length of a `std::array` from its initializers.
    
    ```cpp
        constexpr std::array a1 { 9, 7, 5, 3, 1 }; // The type is deduced to std::array<int, 5>
        constexpr std::array a2 { 9.7, 7.31 };     // The type is deduced to std::array<double, 2>
    ```
    
    - **Omitting just the array length using `std::to_array`**
        - using `std::to_array` is more expensive than creating a `std::array` directly, because it involves creation of a temporary `std::array` that is then used to copy initialize our desired `std::array`.
        - `std::to_array` should only be used in cases where the type can’t be effectively determined from the initializers, and should be avoided when an array is created many times (e.g., inside a loop).
    
    ```cpp
        constexpr auto myArray1 { std::to_array<int, 5>({ 9, 7, 5, 3, 1 }) }; // Specify type and size
        constexpr auto myArray2 { std::to_array<int>({ 9, 7, 5, 3, 1 }) };    // Specify type only, deduce size
        constexpr auto myArray3 { std::to_array({ 9, 7, 5, 3, 1 }) };         // Deduce type and size
    ```
    
    - **length and indexing**
        - **The length of a `std::array` has type `std::size_t`**
        - `0UZ` is a literal suffix(`std::size_t`)
        - **The length and indices of `std::array` have type `size_type`, which is always `std::size_t`**
            - In the case of `std::array`, `size_type` is *always* an alias for `std::size_t`.
        - in C++17, we can use the `std::size()` non-member function (which for `std::array` just calls the `size()` member function, thus returning the length as unsigned `size_type`).
        - **Subscripting `std::array` using `operator[]` or the `at()` member function**
        - **`std::get()` does compile-time bounds checking for constexpr indices**
        
        ```cpp
            std::cout << std::get<3>(prime); //print the value of element with index 3
            std::cout << std::get<9>(prime); //invalid index (compile error)
        ```
        
    - **Passing and returning std::array**
        - **Using function templates to pass `std::array` of different element types or lengths**
        
        ```cpp
        template<typename T, std::size_t N> // N is a non-type template parameter
        struct array;
        
        template <typename T, std::size_t N> // note that this template parameter declaration matches the one for std::array
        void passByRef(const std::array<T, N>& arr)
        {
            static_assert(N != 0); // fail if this is a zero-length std::array
        
            std::cout << arr[0] << '\n';
        }
        ```
        
        - **Auto non-type template parameters**
        
        ```cpp
        template <typename T, auto N> // now using auto to deduce type of N
        void passByRef(const std::array<T, N>& arr)
        {
            static_assert(N != 0); // fail if this is a zero-length std::array
        
            std::cout << arr[0] << '\n';
        }
        ```
        
        - Static assert on std::array
    - **Returning a `std::array`**
        - **Returning a `std::array` by value**
            - The array isn’t huge.
            - The element type is cheap to copy (or move).
            - The code isn’t being used in a performance-sensitive context.
        - **Returning a `std::array` via an out parameter**
            - Pass by reference
        - **Returning a `std::vector` instead**
            - `std::vector` is move-capable and can be returned by value without making expensive copies.
    - **Defining and assigning to a `std::array` of structs**
        
        ```cpp
        struct House
        {
            int number{};
            int stories{};
            int roomsPerStory{};
        };
        
        int main()
        {
            std::array<House, 3> houses{};
        
            houses[0] = { 13, 1, 7 };
            houses[1] = { 14, 2, 5 };
            houses[2] = { 15, 2, 4 };
            
        	      constexpr std::array houses { // use CTAD to deduce template arguments <House, 3>
                    House{ 13, 1, 7 },
                    House{ 14, 2, 5 },
                    House{ 15, 2, 4 }
                };
                
             // This works as expected
        constexpr std::array<House, 3> houses { // initializer for houses
            { // extra set of braces to initialize the C-style array member with implementation_defined_name
                { 13, 4, 30 }, // initializer for array element 0
                { 14, 3, 10 }, // initializer for array element 1
                { 15, 3, 40 }, // initializer for array element 2
             }
        };
        ```
        
        - When initializing a `std::array` with a struct, class, or array and not providing the element type with each initializer, you’ll need an extra pair of braces so that the compiler will properly interpret what to initialize.
    - **Brace elision for aggregates**
        - **brace elision**, which lays out some rules for when multiple braces may be omitted.
        - Generally, you can omit braces when initializing a `std::array` with scalar (single) values, or when initializing with class types or arrays where the type is explicitly named with each element.
        - There is no harm in always initializing `std::array` with double braces, as it avoids having to think about whether brace-elision is applicable in a specific case or not.
    - **Making two-dimensional `std::array` easier using an alias templates**
        
        ```cpp
        using Array2dint34 = std::array<std::array<int, 4>, 3>;
        
        // An alias template for a two-dimensional std::array
        template <typename T, std::size_t Row, std::size_t Col>
        using Array2d = std::array<std::array<T, Col>, Row>;
        ```
        
        - **std::mdspan**
            - Introduced in C++23, `std::mdspan` is a modifiable view that provides a multidimensional array interface for a contiguous sequence of elements
                - `std::mdspan` is not just a read-only view (like `std::string_view`) -- if the underlying sequence of elements is non-const, those elements can be modified.
            
            ```cpp
            		// Define a two-dimensional span into our one-dimensional array
            		// We must pass std::mdspan a pointer to the sequence of elements
            		// which we can do via the data() member function of std::array or std::vector
                std::mdspan mdView { arr.data(), 3, 4 };
            
                // print array dimensions
                // std::mdspan calls these extents
                std::size_t rows { mdView.extents().extent(0) };
                std::size_t cols { mdView.extents().extent(1) };
                std::cout << "Rows: " << rows << '\n';
                std::cout << "Cols: " << cols << '\n';
            
                // print array in 1d
                // The data_handle() member gives us a pointer to the sequence of elements
                // which we can then index
                for (std::size_t i=0; i < mdView.size(); ++i)
                    std::cout << mdView.data_handle()[i] << ' ';
                std::cout << '\n';
            ```
            
            - In C++23, `operator[]` accepts multiple indices, so we use `[row, col]` as our index instead of `[row][col]`.
        - C++26 will include `std::mdarray`, which essentially combines `std::array` and `std::mdspan` into an owning multidimensional array!
- **std::reference_wrapper**
    - a standard library class template that lives in the <functional> header. It takes a type template argument T, and then behaves like a modifiable lvalue reference to T.
        - `Operator=` will reseat a `std::reference_wrapper` (change which object is being referenced).
        - `std::reference_wrapper<T>` will implicitly convert to `T&`.
        - The `get()` member function can be used to get a `T&`. This is useful when we want to update the value of the object being referenced.
- **std::array and enumerations**
    - **Using static assert to ensure the proper number of array initializers**
    - **Using constexpr arrays for better enumeration input and output**
        - This allows us to do two things:
            1. Index the array using the enumerator’s value to get the name of that enumerator.
            2. Use a loop to iterate through all of the names, and be able to correlate a name back to the enumerator based on index.
    - **Range-based for-loops and enumerations**
        - create a `constexpr std::array` containing each of our enumerators, and then iterate over that.
- **C-style arrays**
    
    ```cpp
    int testScore[30] {};      // Defines a C-style array named testScore that contains 30 value-initialized int elements (no include required)
    ```
    
    - C-style arrays dynamically allocated on the heap are allowed to have length 0.
    - **The array length of a c-style array must be a constant expression**
    - C-style arrays will accept signed or unsigned indexes (or unscoped enumerations).
    - Just like `std::array`, C-style arrays are aggregates, which means they can be initialized using aggregate initialization.
        - Prefer omitting the length of a C-style array when explicitly initializing every array element with a value.
    - Applied to a C-style array, `sizeof()` returns the number of bytes used by the entire array
    - In C++17, we can use `std::size()` (defined in the <iterator> header), which returns the array length as an unsigned integral value (of type `std::size_t`).
        - In C++20, we can also use `std::ssize()`, which returns the array length as a signed integral value (of a large signed integral type, probably `std::ptrdiff_t`).
    - **C-style arrays don’t support assignment**
    - **Array to pointer conversions (array decay)**
        - In most cases, when a C-style array is used in an expression, the array will be implicitly converted into a pointer to the element type, initialized with the address of the first element (with index 0). Colloquially, this is called **array decay** (or just **decay** for short).
        - In C++, there are a few common cases where an C-style array doesn’t decay:
            1. When used as an argument to `sizeof()` or `typeid()`.
            2. When taking the address of the array using `operator&`.
            3. When passed as a member of a class type.
            4. When passed by reference.
        - C-style arrays are passed by address, even when it looks like they are passed by value.
        - Two C-style arrays with the same element type but different lengths will decay into the same pointer type.
        - A function parameter expecting a C-style array type should use the array syntax (e.g. `int arr[]`) rather than pointer syntax (e.g. `int *arr`).
    - Avoid C-style arrays whenever practical.
    - **Multidimensional C-style Arrays**
        
        ```cpp
        int threedee[4][4][4]; // a 4x4x4 array (an array of 4 arrays of 4 arrays of 4 ints)
        ```
        
        - C++ uses **row-major order**, where elements are sequentially placed in memory row-by-row, ordered from left to right, top to bottom
        
        ```cpp
        	1. ->
        |	  [0][0] [0][1] [0][2] [0][3] [0][4] 
        v	  [1][0] [1][1] [1][2] [1][3] [1][4] 
        2.	[2][0] [2][1] [2][2] [2][3] [2][4]
        ```
        
        - Some other languages (like Fortran) use **column-major order**, elements are sequentially placed in memory column-by-column, from top to bottom, left to right
- **Pointer arithmetic and subscripting**
    - **Pointer arithmetic** is a feature that allows us to apply certain integer arithmetic operators (addition, subtraction, increment, or decrement) to a pointer to produce a new memory address.
    - Given some pointer `ptr`, `ptr + 1` returns the address of the *next object* in memory (based on the type being pointed to).
    - **Subscripting is implemented via pointer arithmetic**
        - the compiler converts `ptr[n]` into `*((ptr) + (n))`
        - we can also subscript a pointer as `n[ptr]`
    - **Pointer arithmetic and subscripting are relative addresses**
- **C-style strings**
    - To define a C-style string variable, simply declare a C-style array variable of `char` (or `const char` / `constexpr char`)
    
    ```cpp
    char str1[8]{};                    // an array of 8 char, indices 0 through 7
    
    const char str2[]{ "string" };     // an array of 7 char, indices 0 through 6
    constexpr char str3[] { "hello" }; // an array of 6 const char, indices 0 through 5
    ```
    
    - **C-style strings will decay**
    - **Array overflow** or **buffer overflow** is a computer security issue that occurs when more data is copied into storage than the storage can hold.
        - In such cases, the memory just beyond the storage will be overwritten, leading to undefined behavior.
    - In such cases, the memory just beyond the storage will be overwritten, leading to undefined behavior.
    - `strlen()` will work on decayed arrays, and returns the length of the string being held, excluding the null terminator
    - Avoid non-const C-style string objects in favor of `std::string`
    - **C-style string symbolic constants**
        
        ```cpp
        auto s1{ "Alex" };  // type deduced as const char*
        auto* s2{ "Alex" }; // type deduced as const char*
        auto& s3{ "Alex" }; // type deduced as const char(&)[5]
        ```
        
        - Avoid C-style string symbolic constants in favor of `constexpr std::string_view`.
-