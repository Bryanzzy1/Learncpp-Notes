---
tags:
  - cpp/namespaces
  - cpp/scope
  - concept
  - syntax
aliases:
  - namespace
  - global namespace
  - qualified name
  - using directive
up: "[[Chapter 2 — Functions and Files]]"
related:
  - "[[Functions]]"
  - "[[Preprocessing]]"
  - "[[Header Files]]"
---

# Namespaces

A named scope region that keeps identifiers distinct from identically named identifiers in other scopes.

```cpp
namespace myApp {
    int value { 5 };     // accessed as myApp::value
}
int value { 10 };        // global namespace — no prefix needed
```

**Global namespace:**
- Any name not defined inside a class, function, or namespace belongs implicitly to the global namespace
- Identifiers in the global namespace are accessible without a prefix

**Qualified names:**
- Including the namespace prefix (`std::cout`) makes an identifier a **qualified name**
- `::` is the scope resolution operator

**`using`-directives:**
- `using namespace std;` imports all `std` names into the current scope
- Avoid `using`-directives at file scope or in header files — causes name collisions and pollutes every file that includes the header

> Full coverage: [[Chapter 2 — Functions and Files]] → Namespace
