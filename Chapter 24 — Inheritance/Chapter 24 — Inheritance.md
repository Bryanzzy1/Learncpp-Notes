---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Inheritance
  - Ch24
up: LearnCPP
related:
  - "[[Basic Inheritance]]"
  - "[[Derived Class Constructors]]"
  - "[[Inheritance and Access Specifiers]]"
  - "[[Adding Functionality to Derived Classes]]"
  - "[[Overriding Inherited Functions]]"
  - "[[Hiding Inherited Functionality]]"
  - "[[Multiple Inheritance]]"
  - "[[Mixins]]"
---

# Chapter 24 — Inheritance

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 20, 2026 12:12 PM

```cpp
class Derived : public Base
{
private:
	using Base::m_value;

public:
	Derived(int value) : Base { value }
	{
	}
};
```

## Notes

- **[[Basic Inheritance]]**
    - the class being inherited from is called the **parent class**, **base class**, or **superclass**, and the class doing the inheriting is called the **child class**, **derived class**, or **subclass**.
    
    ```cpp
    // BaseballPlayer publicly inheriting Person
    class BaseballPlayer : public Person
    {
    ```
    
    - We automatically receive the member functions and member variables of the base class through inheritance, and then simply add the additional functions or member variables we want
    - **Order of construction of derived classes**
        - As you can see, when we constructed Derived, the Base portion of Derived got constructed first.
        - C++ constructs derived classes in phases, starting with the most-base class (at the top of the inheritance tree) and finishing with the most-child class (at the bottom of the inheritance tree). As each class is constructed, the appropriate constructor from that class is called to initialize that part of the class.
- **[[Derived Class Constructors]]**
    - Here's what actually happens when base is instantiated:
        1. Memory for base is set aside
        2. The appropriate Base constructor is called
        3. The member initializer list initializes variables
        4. The body of the constructor executes
        5. Control is returned to the caller
    - Here's what actually happens when derived is instantiated:
        1. Memory for derived is set aside (enough for both the Base and Derived portions)
        2. The appropriate Derived constructor is called
        3. **The Base object is constructed first using the appropriate Base constructor**. If no base constructor is specified, the default constructor will be used.
        4. The member initializer list initializes variables
        5. The body of the constructor executes
        6. Control is returned to the caller
    - One of the current shortcomings of our Derived class as written is that there is no way to initialize m_id when we create a Derived object
        
        ```cpp
            Derived(double cost=0.0, int id=0)
                : Base{ id } // Call Base(int) constructor with value id!
                , m_cost{ cost }
            {
            }
        ```
        
        - Fortunately, C++ gives us the ability to explicitly choose which Base class constructor will be called
    - When a derived class is destroyed, each destructor is called in the *reverse* order of construction. In the above example, when c is destroyed, the C destructor is called first, then the B destructor, then the A destructor.
- **[[Inheritance and Access Specifiers]]**
    - The **protected** access specifier allows the class the member belongs to, friends, and derived classes to access the member.
    
    ```cpp
    class Base
    {
    public:
        int m_public {}; // can be accessed by anybody
    protected:
        int m_protected {}; // can be accessed by Base members, friends, and derived classes
    private:
        int m_private {}; // can only be accessed by Base members and friends (but not derived classes)
    };
    ```
    
    - **Different kinds of inheritance, and their impact on access**
        
        ```cpp
        // Inherit from Base publicly
        class Pub: public Base
        {
        };
        
        // Inherit from Base protectedly
        class Pro: protected Base
        {
        };
        
        // Inherit from Base privately
        class Pri: private Base
        {
        };
        
        class Def: Base // Defaults to private inheritance
        {
        };
        ```
        
        - If you do not choose an inheritance type, C++ defaults to private inheritance
        - when members are inherited, the access specifier for an inherited member may be changed (in the derived class only) depending on the type of inheritance used.
            - members that were public or protected in the base class may change access specifiers in the derived class.
        - Public Inheritance:
            - Public inherited members stay public
            - Protected inherited members stay protected
            - Private inherited members stay inaccessible
            - Use public inheritance unless you have a specific reason to do otherwise.
        - Private Inheritance:
            - Public inherited members become private
            - Protected inherited members become private
            - Private inherited members stay inaccessible
        - With protected inheritance, the public and protected members become protected, and private members stay inaccessible.
            - Use very rarely
    - First, a class (and friends) can always access its own non-inherited members. The access specifiers only affect whether outsiders and derived classes can access those members.
    - Second, when derived classes inherit members, those members may change access specifiers in the derived class.
    
    | **Access specifier in base class** | **Access specifier when inherited publicly** | **Access specifier when inherited privately** | **Access specifier when inherited protectedly** |
    | --- | --- | --- | --- |
    | Public | Public | Private | Protected |
    | Protected | Protected | Private | Protected |
    | Private | Inaccessible | Inaccessible | Inaccessible |
- **[[Adding Functionality to Derived Classes]]**
    - Just define the function in the derived
- **[[Overriding Inherited Functions]]**
    - You can redefined base function by overriding it in the derived class
    - Adding to existing functionalities:
        
        ```cpp
        class Base
        {
        public:
            Base() { }
        
            void identify() const { std::cout << "Base::identify()\n"; }
        };
        
        class Derived: public Base
        {
        public:
            Derived() { }
        
            void identify() const
            {
                std::cout << "Derived::identify()\n";
                Base::identify(); // note call to Base::identify() here
            }
        };
        ```
        
        - Calling function `identify()` without a scope resolution qualifier would default to the `identify()` in the current class, which would be `Derived::identify()`
    - **Overload resolution in derived classes**
        
        ```cpp
        class Base
        {
        public:
            void print(int)    { std::cout << "Base::print(int)\n"; }
            void print(double) { std::cout << "Base::print(double)\n"; }
        };
        
        class Derived: public Base
        {
        public:
            using Base::print; // make all Base::print() functions eligible for overload resolution
            void print(double) { std::cout << "Derived::print(double)"; }
        };
        
        int main()
        {
            Derived d{};
            d.print(5); // calls Base::print(int), which is the best matching function visible in Derived
        
            return 0;
        }
        ```
        
- **[[Hiding Inherited Functionality]]**
    
    ```cpp
    class Derived : public Base
    {
    private:
    	using Base::m_value;
    
    public:
    	Derived(int value) : Base { value }
    	{
    	}
    };
    ```
    
    - if a Base class has a public virtual function, and the Derived class changes the access specifier to private, the public can still access the private Derived function by casting a Derived object to a Base& and calling the virtual function.
    
    ```cpp
    
    class A
    {
    public:
        virtual void fun()
        {
            std::cout << "public A::fun()\n";
        }
    };
    
    class B : public A
    {
    private:
        virtual void fun()
        {
             std::cout << "private B::fun()\n";
       }
    };
    ```
    
    - **Deleting functions in the derived class**
        - `int getValue() const = delete; // mark this function as inaccessible`
- **[[Multiple Inheritance]]**
    - Enables a derived class to inherit members from more than one parent.
    
    ```cpp
    // Teacher publicly inherits Person and Employee
    class Teacher : public Person, public Employee
    {
    private:
        int m_teachesGrade{};
    
    public:
        Teacher(std::string_view name, int age, std::string_view employer, double wage, int teachesGrade)
            : Person{ name, age }, Employee{ employer, wage }, m_teachesGrade{ teachesGrade }
        {
        }
    };
    ```
    
    - **[[Mixins]]**
        - a small class that can be inherited from in order to add properties to a class.
        
        ```cpp
        class Box // mixin Box class
        {
        public:
        	void setTopLeft(Point2D point) { m_topLeft = point; }
        	void setBottomRight(Point2D point) { m_bottomRight = point; }
        private:
        	Point2D m_topLeft{};
        	Point2D m_bottomRight{};
        };
        ```
        
        - a derived class can inherit from a mixin base class using the derived class as a template type parameter. Such inheritance is called **Curiously Recurring Template Pattern** (CRTP for short), which looks like this:
            
            ```cpp
            template <class T>
            class Mixin
            {
                // Mixin<T> can use template type parameter T to access members of Derived
                // via (static_cast<T*>(this))
            };
            
            class Derived : public Mixin<Derived>
            {
            };
            ```
            
    - Avoid multiple inheritance unless alternatives lead to more complexity.
