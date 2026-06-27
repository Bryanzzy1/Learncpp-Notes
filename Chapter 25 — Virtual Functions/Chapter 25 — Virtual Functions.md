---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/inheritance
  - cpp/polymorphism
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Virtual Functions
  - Ch25
up: LearnCPP
related:
  - "[[Pointers and References to Base]]"
  - "[[Virtual Functions]]"
  - "[[Virtual Destructors]]"
  - "[[Printing with Virtual Dispatch]]"
  - "[[Polymorphism]]"
  - "[[Override and Final Specifiers]]"
  - "[[Early Binding and Late Binding]]"
  - "[[Virtual Table]]"
  - "[[Pure Virtual Functions]]"
  - "[[Interface Classes]]"
  - "[[Virtual Base Classes]]"
  - "[[Object Slicing]]"
  - "[[Dynamic Casting]]"
---

# Chapter 25 — Virtual Functions

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 24, 2026 3:23 PM

## Notes

- **[[Pointers and References to Base]]**
    
    ```cpp
        // These are both legal!
        Base& rBase{ derived }; // rBase is an lvalue reference (not an rvalue reference)
        Base* pBase{ &derived };
    ```
    
    - Since Derived has a Base part
    - Calling `rBase.getName()` calls `Base::getName()`, not `Derived::getName()`, even though the underlying object is a `Derived`.
        - The reason is that `Derived::getName()` *shadows* (hides) `Base::getName()` only for `Derived`-typed access.
        - This resolution happens based on the *type of the pointer/reference*, not the actual object type.
        - This is called static (compile-time) dispatch.
- **[[Virtual Functions]]**
    - A **virtual function** is a special type of member function that, when called, resolves to the most-derived version of the function for the actual type of the object being referenced or pointed to.
        - A derived function is considered a match if it has the same signature (name, parameter types, and whether it is const) and return type as the base version of the function. Such functions are called **overrides**.
    
    ```cpp
    class Base
    {
    public:
        virtual std::string_view getName() const { return "Base"; } // note addition of virtual keyword
    };
    
    class Derived: public Base
    {
    public:
        virtual std::string_view getName() const { return "Derived"; }
    };
    ```
    
    - Virtual function resolution only works when a member function is called through a pointer or reference to a class type object.
    - Under normal circumstances, the return type of a virtual function and its override must match.
    - If a function is virtual, all matching overrides in derived classes are implicitly virtual.
    - Never call virtual functions from constructors or destructors.
    - Resolving a virtual function call takes longer than resolving a regular one.
        - To make virtual functions work, the compiler has to allocate an extra pointer for each object of a class that has virtual functions. This adds overhead to objects that otherwise have a small size.
    - **[[Virtual Destructors]]**
        - Whenever you are dealing with inheritance, you should make any explicit destructors virtual.
        - Virtualizing the assignment operator is not recommended — leave assignments non-virtual.
        - Very rarely you may want to ignore the virtualization of a function; use the scope resolution operator to call the base from a derived pointer.
        - If a class isn't explicitly designed to be a base class, then it's generally better to have no virtual members and no virtual destructor.
        - If a class is designed to be used as a base class and/or has any virtual functions, then it should always have a virtual destructor.
        - Herb Sutter: "A base class destructor should be either public and virtual, or protected and non-virtual."
- **[[Polymorphism]]**
    - The ability of an entity to have multiple forms.
    - **Compile-time polymorphism** refers to forms of polymorphism that are resolved by the compiler. These include function overload resolution and template resolution.
    - **Runtime polymorphism** refers to forms of polymorphism that are resolved at runtime. This includes virtual function resolution.
- **[[Override and Final Specifiers]]**
    - Use the `override` specifier (but not the `virtual` keyword) on override functions in derived classes. This includes virtual destructors.
        - If a member function is both `const` and an `override`, the `const` must be listed first: `const override` is correct, `override const` is not.
    - **The final specifier**
        - The `final` specifier prevents overriding a virtual function or inheriting from a class.
        - If you do not intend your class to be inherited from, mark your class as `final`.
    - **Covariant return types**
        - If the return type of a virtual function is a pointer or a reference to some class, override functions can return a pointer or a reference to a derived class. These are called **covariant return types**.
- **[[Early Binding and Late Binding]]**
    - **Binding and dispatching**
        - **Binding** is the process of associating names with properties.
        - **Function binding** determines what function definition is associated with a function call.
        - The process of actually invoking a bound function is called **dispatching**.
    - **Early binding** (static binding): resolved at compile-time; the compiler generates a direct jump to the function address.
    - **Late binding** (dynamic dispatch): resolved at runtime; requires an extra level of indirection (e.g. via function pointers or vtable).
- **[[Virtual Table]]**
    - A lookup table of functions used to resolve function calls in a dynamic/late binding manner.
    - Every class that uses virtual functions has a corresponding virtual table — a static array set up at compile time.
    - The compiler adds a hidden `*__vptr` pointer to each object; it points to the virtual table for that class.
    - Each vtable entry points to the most-derived version of the function that objects of that class are allowed to call.
- **[[Pure Virtual Functions]]**
    - A **pure virtual function** has no body: `virtual int getValue() const = 0;`
    - Any class with one or more pure virtual functions becomes an **abstract base class** and cannot be instantiated.
    - Pure virtual functions can optionally have a definition (body outside the class), providing a default that derived classes may call explicitly.
    - **[[Interface Classes]]**
        - An **interface class** has no member variables and all functions are pure virtual.
        - Interface classes define the functionality that derived classes must implement.
        - Conventionally named beginning with `I` (e.g. `IErrorLog`).
- **[[Virtual Base Classes]]**
    - Solves the diamond problem in multiple inheritance by ensuring only one copy of the shared base exists.
    - Use the `virtual` keyword in the inheritance list: `class Scanner: virtual public PoweredDevice`.
    - The most-derived class is responsible for constructing the virtual base directly.
    - Adds vtable overhead even without virtual functions, so the subobjects can locate the shared base at runtime.
- **[[Object Slicing]]**
    - Assigning a Derived object to a Base object copies only the Base portion — the derived members are "sliced off".
    - Much more likely to occur accidentally when passing by value to functions.
- **[[Dynamic Casting]]**
    - `dynamic_cast` converts a base class pointer/reference to a derived class pointer/reference (downcasting).
    - On failure: returns `nullptr` for pointer casts; throws `std::bad_cast` for reference casts.
    - Always check for `nullptr` after a pointer `dynamic_cast`.
    - Relies on RTTI (Run-time type information); can be disabled by compiler flags, breaking `dynamic_cast`.
    - Prefer virtual functions over downcasting; use `static_cast` for downcasting only when you are certain of the type.
- **[[Printing with Virtual Dispatch]]**
    - `operator<<` cannot be made virtual (it is a non-member function).
    - Pattern: implement `operator<<` as a `friend` in the base class that delegates to a virtual `print()` member function — virtual dispatch then picks the correct derived override.
    - A more flexible variant has `print()` accept the `std::ostream&` directly, enabling full stream access within each override.
