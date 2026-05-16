---
tags:
  - cpp/pointers
  - cpp/functions
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - pass by address
  - pass by pointer
up: "[[Pointers]]"
related:
  - "[[Pointers]]"
  - "[[Lvalue References]]"
  - "[[Pass by Reference]]"
  - "[[In and Out Parameters]]"
  - "[[Functions]]"
  - "[[Static Local Variables]]"
  - "[[Chapter 12 — References and Pointers]]"
---

# Pass by Address

**Pass by address** passes a pointer (the address of an object) rather than the object itself. The function receives a pointer parameter and dereferences it to access the object.

```cpp
void print(const int* ptr)
{
    if (ptr)               // always check for null before dereferencing
        std::cout << *ptr << '\n';
}

int x { 5 };
print(&x);                 // caller passes the address of x
```

The key advantage over [[Pass by Reference]]: a pointer can be `nullptr`, allowing the caller to signal "no value."

## Guidelines

- **Prefer pointer-to-const parameters** (`const T*`) unless the function must modify the pointed-to object.
- **Do not make the pointer parameter itself const** (`T* const`) without a specific reason — the pointer is copied by value anyway, so const on the pointer doesn't protect the caller.
- **Prefer pass by reference over pass by address** in most cases. Only choose pass by address when nullable semantics are needed.

## Passing pointers by reference

To let a function modify the caller's *pointer itself* (e.g. set it to `nullptr` or a new address), pass the pointer by reference:

```cpp
void nullify(int*& refptr)  // refptr is a reference to a pointer
{
    refptr = nullptr;       // modifies the caller's pointer variable
}
```

## Don't return non-const static local variables by reference

Returning a reference or pointer to a non-const [[Static Local Variables|static local variable]] is legal but error-prone — every call returns a handle to the same shared object, which is surprising. Prefer returning values instead.

> Full coverage: [[Chapter 12 — References and Pointers]] → Pointers
