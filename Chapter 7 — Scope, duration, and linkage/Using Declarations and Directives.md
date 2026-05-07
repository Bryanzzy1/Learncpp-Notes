---
tags:
  - cpp/namespaces
  - concept
  - syntax
  - best-practice
  - subnode
aliases:
  - using declaration
  - using directive
  - qualified name
  - unqualified name
  - using namespace std
up: "[[User-Defined Namespaces]]"
related:
  - "[[User-Defined Namespaces]]"
  - "[[Unnamed and Inline Namespaces]]"
  - "[[Namespaces]]"
  - "[[IO]]"
---

# Using Declarations and Directives

## Qualified vs unqualified names

| Term | Definition | Example |
|------|-----------|---------|
| **Qualified name** | Includes an associated scope | `std::cout` |
| **Unqualified name** | No scoping qualifier | `cout` |

## Using declaration

Allows one specific unqualified name as an alias for a qualified name:

```cpp
using std::cout;    // only cout is brought in
cout << "Hello\n";
```

## Using directive

Brings **all** identifiers from a namespace into scope:

```cpp
using namespace std;   // everything in std is accessible unqualified
```

## Comparison

| | Using declaration | Using directive |
|--|--|--|
| Syntax | `using std::cout;` | `using namespace std;` |
| Scope | Single identifier | Entire namespace |
| Risk of name collision | Low | Higher |

## Best practices

- **Do not use using-statements in header files** or before an `#include` directive — they affect all files that include the header. This defeats the purpose of [[Namespaces]].
- Once a using-statement is declared, it cannot be cancelled or replaced within the same scope.
- Prefer using declarations over using directives (e.g., `using std::cout` rather than `using namespace std` when working with [[IO]]).

> Full coverage: [[Chapter 7 — Scope, duration, and linkage]] → Using Declarations and Directives
