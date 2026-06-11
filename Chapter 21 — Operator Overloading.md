# Chapter 21 — Operator Overloading

Pin: No
Fav: No
Category: Done (https://app.notion.com/p/Done-24fb13b569b5836bb160816e5fa04870?pvs=21)
Created: April 13, 2026 8:45 PM
Last edited: June 11, 2026 3:14 PM

## Notes

- **Operator Overloading**
    - If all of the operands are fundamental data types, the compiler will call a built-in routine if one exists. If one does not exist, the compiler will produce a compiler error.
    - If *any* of the operands are program-defined types (e.g. one of your classes, or an enum type), the compiler will use the function overload resolution algorithm
        - It may also involve implicitly converting program-defined types into fundamental types (via an overloaded typecast,
    - You can only overload the operators that exist.
    - At least one of the operands in an overloaded operator must be a user-defined type. This means you could overload `operator+(int, Mystring)`, but not `operator+(int, double)`.
    - It is not possible to change the number of operands an operator supports.
    - When overloading operators, it’s best to keep the function of the operators as close to the original intent of the operators as possible.
    - Operators that do not modify their operands (e.g. arithmetic operators) should generally return results by value.
        - Operators that modify their leftmost operand (e.g. pre-increment, any of the assignment operators) should generally return the leftmost operand by reference.
- **Overloading the arithmetic operators using friend functions**
    - Overloading operators using friend functions
    - Friend functions can be defined inside the class
- **Overloading operators using normal functions**
    - Similar to the friend function but you don’t get access to the private memebers, so use a access function.
    - Prefer overloading operators as normal functions instead of friends if it’s possible to do so without adding additional functions.
- **Overloading the I/O operators**
    
    ```cpp
    std::ostream& operator<< (std::ostream& out, const Point& point)
    {
        // Since operator<< is a friend of the Point class, we can access Point's members directly.
        out << "Point(" << point.m_x << ", " << point.m_y << ", " << point.m_z << ')'; // actual output done here
    
        return out; // return std::ostream so we can chain calls to operator<<
    }
    ```
    
    - It is also possible to overload the input operator (Operator >>)
        - The key thing you need to know is that `std::cin` is an object of type `std::istream`.
        
        ```cpp
        // note that point must be non-const so we can modify the object
        std::istream& operator>> (std::istream& in, Point& point)
        {
            // This version subject to partial extraction issues (see below)
            in >> point.m_x >> point.m_y >> point.m_z;
        
            return in;
        }
        ```
        
        - **Guarding against partial extraction**
            - A **transactional operation** must either completely succeed or completely fail
            
            ```cpp
            // note that point must be non-const so we can modify the object
            // note that this implementation is a non-friend
            std::istream& operator>> (std::istream& in, Point& point)
            {
                double x{};
                double y{};
                double z{};
            
                in >> x >> y >> z;
                point = in ? Point{x, y, z} : Point{};
            
                return in;
            }
            ```
            
        - Also need to verify/handle invalid semantic input
- **Overloading operators using member functions**
    - When overloading an operator using a member function:
        - The overloaded operator must be added as a member function of the left operand.
        - The left operand becomes the implicit *this object
        - All other operands become function parameters.
        
        ```cpp
        // note: this function is a member function!
        // the cents parameter in the friend version is now the implicit *this parameter
        Cents Cents::operator+ (int value) const
        {
            return Cents { m_cents + value };
        }
        ```
        
    - The left parameter is removed, because that parameter now becomes the implicit *this object.
    - **Not everything can be overloaded as a friend function**
        - The assignment (=), subscript ([]), function call (()), and member selection (->) operators must be overloaded as member functions, because the language requires them to be.
    - **Not everything can be overloaded as a member function**
        - not able to overload operator<< as a member function.
- **When to use a normal, friend, or member function overload:**
    - If you’re overloading assignment (=), subscript ([]), function call (()), or member selection (->), do so as a member function.
    - If you’re overloading a unary operator, do so as a member function.
    - If you’re overloading a binary operator that does not modify its left operand (e.g. operator+), do so as a normal function (preferred) or friend function.
    - If you’re overloading a binary operator that modifies its left operand, but you can’t add members to the class definition of the left operand (e.g. operator<<, which has a left operand of type ostream), do so as a normal function (preferred) or friend function.
    - If you’re overloading a binary operator that modifies its left operand (e.g. operator+=), and you can modify the definition of the left operand, do so as a member function.
- **Overloading the comparison operators**
    - Only define overloaded operators that make intuitive sense for your class.
    - **Minimizing comparative redundancy**
        - operator!= can be implemented as !(operator==)
        - operator> can be implemented as operator< with the order of the parameters flipped
        - operator>= can be implemented as !(operator<)
        - operator<= can be implemented as !(operator>)
    - **The spaceship operator <=> (c++ 20)**
        - Allows us to reduce the number of comparison functions we need to write down to 2 at most, and sometimes just 1!
        - Three way comparision
            - Return < 0 if left is smaller
            - > 0 if right is smaller
            - == 0 if equal
- **Overloading the increment and decrement operators**
    - **Overloading prefix increment and decrement**
        
        ```cpp
        Digit& Digit::operator++()
        {
            // If our number is already at 9, wrap around to 0
            if (m_digit == 9)
                m_digit = 0;
            // otherwise just increment to next number
            else
                ++m_digit;
        
            return *this;
        }
        
        Digit& Digit::operator--()
        {
            // If our number is already at 0, wrap around to 9
            if (m_digit == 0)
                m_digit = 9;
            // otherwise just decrement to next number
            else
                --m_digit;
        
            return *this;
        }
        ```
        
        - Can differentiate overload with number of parameters
        
        ```cpp
            Digit& operator++(); // prefix has no parameter
            Digit& operator--(); // prefix has no parameter
        
            Digit operator++(int); // postfix has an int parameter
            Digit operator--(int); // postfix has an int parameter
        ```
        
    - Note that the prefix and postfix operators do the same job -- they both increment or decrement the object.
- **Overloading the subscript operator**
    - **Overloading operator[]**
        
        ```cpp
            int& operator[] (int index)
            {
                return m_list[index];
            }
        ```
        
        - C++23 adds support for overloading operator[] with multiple subscripts.
        - **Operator[] returns a reference**
        - **Overloaded operator[] for const objects**
            - we can define a non-const and a const version of operator[] separately
            
            ```cpp
                // For non-const objects: can be used for assignment
                int& operator[] (int index)
                {
                    return m_list[index];
                }
            
                // For const objects: can only be used for access
                // This function could also return by value if the type is cheap to copy
                const int& operator[] (int index) const
                {
                    return m_list[index];
                }
            ```
            
            - C++ 23
                
                ```cpp
                // Use an explicit object parameter (self) and auto&& to differentiate const vs non-const
                    auto&& operator[](this auto&& self, int index)
                    {
                        // Complex code goes here
                        return self.m_list[index];
                    }
                ```
                
        - Detect index validity
        - Pointers to objects and overloaded operator[] don’t mix
        - **The function parameter does not need to be an integral type**
- **Overloading the parenthesis operator**
    - The parenthesis operator (operator()) is a particularly interesting operator in that it allows you to vary both the type AND number of parameters it takes.
    - Operator() is also commonly overloaded to implement **functors** (or **function object**), which are classes that operate like functions.
        - The advantage of a functor over a normal function is that functors can store data in member variables (since they are classes).
- **Overloading typecasts**
    - Such a typecast can be used explicitly (via a cast) or implicitly by the compiler to perform conversions as needed.
    
    ```cpp
        // Overloaded int cast
        operator int() const { return m_cents; }
    ```
    
    - You can provide overloaded typecasts for any data type you wish, including your own program-defined data types
    
    ```cpp
        // Allow us to convert Dollars into Cents
        operator Cents() const { return Cents { m_dollars * 100 }; }
    ```
    
    - **Explicit typecasts**
        - Just like single-parameter converting constructors should be marked as explicit, typecasts should be marked as explicit, except in cases where the type to be converted to is essentially synonymous with the destination type.
    - When possible, prefer converting constructors, and avoid overloaded typecasts.
    - When you need to define how convert type A to type B:
        - If B is a class type you can modify, prefer using a converting constructor to create B from A.
        - Otherwise, if A is a class type you can modify, use an overloaded typecast to convert A to B.
        - Otherwise use a non-member function to convert A to B.
- **Overloading the assignment operator**
    - **Copy assignment vs Copy constructor**
        - the copy constructor initializes new objects, whereas the assignment operator replaces the contents of existing objects.
    - The copy assignment operator must be overloaded as a member function.
        
        ```cpp
        // Already forward declared within the class
        
        // A simplistic implementation of operator= (see better implementation below)
        Fraction& Fraction::operator= (const Fraction& fraction)
        {
            // do the copy
            m_numerator = fraction.m_numerator;
            m_denominator = fraction.m_denominator;
        
            // return the existing object so we can chain this operator
            return *this;
        }
        ```
        
    - **self-assignment**
        - In cases where an assignment operator needs to dynamically assign memory, self-assignment can actually be dangerous
        - **Detecting and handling self-assignment**
            
            ```cpp
            // self-assignment check
            	if (this == &str)
            		return *this;
            ```
            
        - Typically the self-assignment check is skipped for copy constructors.
            - Because the object being copy constructed is newly created, the only case where the newly created object can be equal to the object being copied is when you try to initialize a newly defined object with itself
        - **The implicit copy assignment operator**
            - Unlike other operators, the compiler will provide an implicit public copy assignment operator for your class if you do not provide a user-defined one.
            
            ```cpp
                // Default constructor
                Fraction(int numerator = 0, int denominator = 1)
                    : m_numerator { numerator }, m_denominator { denominator }
                {
                    assert(denominator != 0);
                }
            ```
            
- **Shallow vs. deep copying**
    - The default copy constructor and default assignment operators it provides use a copying method known as a memberwise copy (also known as a **shallow copy**).
    - A **deep copy** allocates memory for the copy and then copies the actual value, so that the copy lives in distinct memory from the source.
- **Overloading operators and function templates**
    
    ```cpp
    template <typename T>
    const T& max(const T& x, const T& y)
    {
    		//we can overload the comparision operator < within our class
    		//or any operator that we need to use within this
        return (x < y) ? y : x;
    }
    
    ```