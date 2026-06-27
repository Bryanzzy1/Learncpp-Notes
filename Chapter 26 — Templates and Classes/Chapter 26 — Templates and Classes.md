---
tags:
  - cpp/templates
  - cpp/classes
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Templates and Classes
  - Ch26
up: LearnCPP
related:
  - "[[Template Classes]]"
  - "[[Template Non-type Parameters in Classes]]"
  - "[[Function Template Specialization]]"
  - "[[Class Template Specialization]]"
  - "[[Partial Template Specialization]]"
  - "[[Partial Template Specialization for Pointers]]"
---

# Chapter 26 — Templates and Classes

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 25, 2026 4:21 PM

## Notes

- **[[Template Classes]]**
    - Similar to function templates and can be split to a header and cpp files for defining member functions
- **[[Template Non-type Parameters in Classes]]**
    - **Non-type parameters**
        - An integral type
        - An enumeration type
        - A pointer or reference to a class object
        - A pointer or reference to a function
        - A pointer or reference to a class member function
        - std::nullptr_t
        - A floating point type (since C++20)
    
    ```cpp
    template <typename T, int size> // size is an integral non-type parameter
    class StaticArray
    {
    private:
        // The non-type parameter controls the size of the array
        T m_array[size] {};
    
    public:
        T* getArray();
    
        T& operator[](int index)
        {
            return m_array[index];
        }
    };
    ```
    
    - Non type arguments must be constexpr
- **[[Function Template Specialization]]**
    - **Explicit template specialization** (often shortened to **template specialization**) is a feature that allows us to explicitly define different implementations of a template for specific types or values.
    - When all of the template parameters are specialized, it is called a **full specialization**. When only some of the template parameters are specialized, it is called a **partial specialization**.
    
    ```cpp
    // A full specialization of primary template print<T> for type double
    // Full specializations are not implicitly inline, so make this inline if put in header file
    template<>                          // template parameter declaration containing no template parameters
    void print<double>(const double& d) // specialized for type double
    {
        std::cout << std::scientific << d << '\n';
    }
    ```
    
- **[[Class Template Specialization]]**
    
    ```cpp
    // First define our non-specialized class template
    template <typename T>
    class Storage8
    {
    private:
        T m_array[8];
    
    public:
        void set(int index, const T& value)
        {
            m_array[index] = value;
        }
    
        const T& get(int index) const
        {
            return m_array[index];
        }
    };
    
    // Now define our specialized class template
    template <> // the following is a template class with no templated parameters
    class Storage8<bool> // we're specializing Storage8 for bool
    {
    // What follows is just standard class implementation details
    
    private:
        std::uint8_t m_data{};
    
    public:
        void set(int index, bool value)
        {
            auto mask{ 1 << index };
    
            if (value)
                m_data |= mask;
            else
                m_data &= ~mask;
    	}
    
        bool get(int index)
        {
            auto mask{ 1 << index };
            return (m_data & mask);
        }
    }
    ```
    
    - **Specializing member functions**
        - Same syntax as function template specialization but since explicit function specialization is not implicitly inline, we should mark it as inline if put in a header
    - Specialized classes and functions are often defined in a header file just below the definition of the non-specialized class, so that including a single header includes both the non-specialized class and any specializations.
- **[[Partial Template Specialization]]**
    - allows us to specialize classes (but not individual functions!)
- **[[Partial Template Specialization for Pointers]]**
