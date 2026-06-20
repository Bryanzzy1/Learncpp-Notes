---
tags:
  - cpp/classes
  - cpp/oop
  - concept
  - best-practice
  - chapter
aliases:
  - Object Relationships
  - Ch23
up: LearnCPP
related:
  - "[[Object Composition]]"
  - "[[Composition]]"
  - "[[Aggregation]]"
  - "[[Association]]"
  - "[[Dependencies]]"
  - "[[Container Classes]]"
  - "[[std-initializer-list|std::initializer_list]]"
---

# Chapter 23 — Object Relationships

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: June 17, 2026 11:52 AM

## Notes

- **[[Object Composition]]**
    - There are two basic subtypes of object composition: composition and aggregation.
    - **[[Composition]]**
        - This process of building complex objects from simpler ones is called **object composition**.
            - For this reason, structs and classes are sometimes referred to as **composite types**.
        - To qualify as a **composition**, an object and a part must have the following relationship:
            - The part (member) is part of the object (class)
            - The part (member) can only belong to one object (class) at a time
            - The part (member) has its existence managed by the object (class)
            - The part (member) does not know about the existence of the object (class)
    - **[[Aggregation]]**
        - To qualify as an **aggregation**, a whole object and its parts must have the following relationship:
            - The part (member) is part of the object (class)
            - The part (member) can (if desired) belong to more than one object (class) at a time
            - The part (member) does *not* have its existence managed by the object (class)
            - The part (member) does not know about the existence of the object (class)
        - Consider a car and an engine. A car engine is part of the car. And although the engine belongs to the car, it can belong to other things as well, like the person who owns the car. The car is not responsible for the creation or destruction of the engine.
        - Implement the simplest relationship type that meets the needs of your program, not what seems right in real-life.
        - **std::reference_wrapper**
            - `std::reference_wrapper` is a class that acts like a reference, but also allows assignment and copying, so it's compatible with lists like `std::vector`.
            - When you create your `std::reference_wrapper` wrapped object, the object can't be an anonymous object (since anonymous objects have expression scope, and this would leave the reference dangling).
            - When you want to get your object back out of `std::reference_wrapper`, you use the `get()` member function.
    - Difference:
        - Compositions:
            - Typically use normal member variables
            - Can use pointer members if the class handles object allocation/deallocation itself
            - Responsible for creation/destruction of parts
        - Aggregations:
            - Typically use pointer or reference members that point to or reference objects that live outside the scope of the aggregate class
            - Not responsible for creating/destroying parts
- **[[Association]]**
    - To qualify as an **association**, an object and another object must have the following relationship:
        - The associated object (member) is otherwise unrelated to the object (class)
        - The associated object (member) can belong to more than one object (class) at a time
        - The associated object (member) does *not* have its existence managed by the object (class)
        - The associated object (member) may or may not know about the existence of the object (class)
    - **Associations can be indirect, doesn't have to be pointers**
    - **Reflexive association**
        - objects may have a relationship with other objects of the same type
    - Difference between association, composition, and aggregation
        
        
        | **Property** | **Composition** | **Aggregation** | **Association** |
        | --- | --- | --- | --- |
        | Relationship type | Whole/part | Whole/part | Otherwise unrelated |
        | Members can belong to multiple classes | No | Yes | Yes |
        | Members' existence managed by class | Yes | No | No |
        | Directionality | Unidirectional | Unidirectional | Unidirectional or bidirectional |
        | Relationship verb | Part-of | Has-a | Uses-a |
- **[[Dependencies]]**
    - A **dependency** occurs when one object invokes another object's functionality in order to accomplish some specific task.
- **[[Container Classes]]**
    - a **container class** is a class designed to hold and organize multiple instances of another type (either another class, or a fundamental type).
    - **Value containers** are compositions that store copies of the objects that they are holding (and thus are responsible for creating and destroying those copies).
    - **Reference containers** are aggregations that store pointers or references to other objects (and thus are not responsible for creation or destruction of those objects).
- **[[std-initializer-list|std::initializer_list]]**
    - When a compiler sees an initializer list, it automatically converts it into an object of type std::initializer_list.
    
    ```cpp
    IntArray(std::initializer_list<int> list) // allow IntArray to be initialized via list initialization
    		: IntArray(static_cast<int>(list.size())) // use delegating constructor to set up initial array
    	{
    		// Now initialize our array from the list
    		std::copy(list.begin(), list.end(), m_data);
    	}
    	
    	IntArray array{ 5, 4, 3, 2, 1 }; // initializer list
    ```
    
    - std::initializer_list does not provide access to the elements of the list via subscripting (operator[]).
        - Use range based for loops
        - Use iterator with begin()
    - **List initialization prefers list constructors over non-list constructors**
        - {} over () or list initialization over brace initialization
    - Adding a list constructor to an existing class that did not have one may break existing programs.
    - If you provide list construction, it's a good idea to provide list assignment as well.
    - **Class assignment using std::initializer_list**
        1. Provide an overloaded list assignment operator
        2. Provide a proper deep-copying copy assignment operator
        3. Delete the copy assignment operator
