---
tags:
  - cpp/classes
  - cpp/oop
  - cpp/polymorphism
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - interface class
  - interface
  - abstract interface
  - I-prefix class
up: "[[Pure Virtual Functions]]"
related:
  - "[[Pure Virtual Functions]]"
  - "[[Virtual Functions]]"
  - "[[Virtual Destructors]]"
  - "[[Classes]]"
  - "[[Chapter 25 — Virtual Functions]]"
---

# Interface Classes

An **interface class** is an abstract class that:
- Has **no member variables**.
- Has **only pure virtual functions** (plus a virtual destructor).

It defines a contract — what derived classes must implement — without any data or implementation details.

## Example

```cpp
class IErrorLog
{
public:
    virtual bool openLog(std::string_view filename) = 0;
    virtual bool closeLog() = 0;
    virtual bool writeError(std::string_view errorMessage) = 0;

    virtual ~IErrorLog() {} // virtual destructor so delete through IErrorLog* is safe
};
```

Any class that inherits from `IErrorLog` and overrides all three functions becomes concrete and can be instantiated.

## Naming convention

Interface classes are conventionally prefixed with `I` (e.g. `IErrorLog`, `ISerializable`) to signal that they are pure contracts.

## Benefits

- **Decoupling**: code that depends on `IErrorLog*` works with any implementation.
- **Testability**: swap in a mock implementation during testing.
- **Flexibility**: multiple unrelated classes can implement the same interface without sharing a common base hierarchy.

## Vtable note

Because interface classes have pure virtual functions, their vtable entries typically hold `nullptr` or point to a crash handler (`__purecall`). Derived classes fill in the correct entries upon construction.

Always give interface classes a [[Virtual Destructors|virtual destructor]] so that `delete`-ing through an interface pointer correctly calls the derived destructor.

> Full coverage: [[Chapter 25 — Virtual Functions]] → Pure Virtual Functions → Interface Classes
