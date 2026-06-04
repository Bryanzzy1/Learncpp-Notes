# Chapter 18 - Iterators and Algorithms(In Construction)

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 4, 2026 6:32 PM

## Notes

- **Sorting an array**
    - `std::sort` lives in the <algorithm> header, and can be invoked on an array
        
        ```cpp
        	int array[]{ 30, 50, 20, 10, 40 };
        
        	std::sort(std::begin(array), std::end(array));
        
        ```
        
- **Iterators**
    - An **iterator** is an object designed to traverse through a container (e.g. the values in an array, or the characters in a string), providing access to each element along the way.
    - **Pointers as an iterator**
        
        ```cpp
        std::array arr{ 0, 1, 2, 3, 4, 5, 6 };
        
            auto begin{ &arr[0] };
            // note that this points to one spot beyond the last element
            auto end{ begin + std::size(arr) };
        
            // for-loop with pointer
            for (auto ptr{ begin }; ptr != end; ++ptr) // ++ to move to next element
            {
                std::cout << *ptr << ' '; // Indirection to get value of current element
            }
        ```
        
    - `std::begin` and `std::end` for C-style arrays are defined in the <iterator> header.
        - `std::begin` and `std::end` for containers that support iterators are defined in the header files for those containers (e.g. <array>, <vector>).
    - With iterators, it is conventional to use `operator!=` to test whether the iterator has reached the end element
    - iterators can be left “dangling” if the elements being iterated over change address or are destroyed
        - iterator has been **invalidated**.
    - The `erase()` function returns an iterator to the element one past the erased element
- **standard library algorithms**
    - `std::find` searches for the first occurrence of a value in a container.
        - an iterator to the starting element in the sequence, an iterator to the ending element in the sequence, and a value to search for.
        - returns an iterator pointing to the element (if it is found) or the end of the container (if the element is not found).
    - **Using std::find_if to find an element that matches some condition**
        
        ```cpp
        // Our function will return true if the element matches
        bool containsNut(std::string_view str)
        {
            // std::string_view::find returns std::string_view::npos if it doesn't find
            // the substring. Otherwise it returns the index where the substring occurs
            // in str.
            return str.find("nut") != std::string_view::npos;
        }
        
        // Scan our array to see if any elements contain the "nut" substring
            auto found{ std::find_if(arr.begin(), arr.end(), containsNut) };
        ```
        
    - **Using std::count and std::count_if to count how many occurrences there are**
        
        ```cpp
        bool containsNut(std::string_view str)
        {
        	return str.find("nut") != std::string_view::npos;
        }
        
        int main()
        {
        	std::array<std::string_view, 5> arr{ "apple", "banana", "walnut", "lemon", "peanut" };
        
        	auto nuts{ std::count_if(arr.begin(), arr.end(), containsNut) };
        ```
        
    - **Using std::sort to custom sort**
        
        ```cpp
        bool greater(int a, int b)
        {
            // Order @a before @b if @a is greater than @b.
            return (a > b);
        }
        
        int main()
        {
            std::array arr{ 13, 90, 99, 5, 40, 80 };
        
            // Pass greater to std::sort
            std::sort(arr.begin(), arr.end(), greater);
        ```
        
        - C++ provides a custom type (named `std::greater`
    - **Using std::for_each to do something to all elements of a container**
        
        ```cpp
        void doubleNumber(int& i)
        {
            i *= 2;
        }
        
        int main()
        {
            std::array arr{ 1, 2, 3, 4 };
        
            std::for_each(arr.begin(), arr.end(), doubleNumber);
        ```
        
    - **Performance and order of execution**
        - Before using a particular algorithm, make sure performance and execution order guarantees work for your particular use case.
    - C++20 adds *ranges*, which allow us to simply pass `arr`. This will make our code even shorter and more readable.
    - Favor using functions from the algorithms library over writing your own functionality to do the same thing.
- **Timing your code**
    - **Things that can impact the performance of your program**
        - make sure you’re using a release build target, not a debug build target
        - Make sure your system isn’t doing anything CPU, memory, or hard drive intensive (e.g. playing a game, searching for a file, running an antivirus scan, or installing a update in the background)
        - if your program uses a random number generator, the particular sequence of random numbers generated may impact timing.
        - make sure you’re not timing waiting for user input, as how long the user takes to input something should not be part of your timing considerations
    - **Measuring performance**
        - When measuring the performance of your program, gather at least 3 results.
        -