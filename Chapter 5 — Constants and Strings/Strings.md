---
tags:
  - cpp/strings
  - cpp/types
  - concept
  - syntax
aliases:
  - std::string
  - std::string_view
  - string
  - string_view
  - getline
  - dangling view
  - std::ws
up: "[[Chapter 5 — Constants and Strings]]"
related:
  - "[[IO]]"
  - "[[size_t]]"
  - "[[Char]]"
  - "[[Literals]]"
  - "[[static_cast]]"
  - "[[getline]]"
---

# Strings — std::string and std::string_view

## std::string

`#include <string>` — owns a variable-length string and manages its memory.

```cpp
std::string name { "Alex" };
name.length();   // returns std::size_t — cast with static_cast<int> to avoid sign warnings
```

**Input:**
- `operator>>` stops at the first whitespace — use [[getline|`std::getline()`]] for a full line
- `std::ws` discards leading whitespace before extraction:

```cpp
std::getline(std::cin >> std::ws, name);
```

**String literal suffix:** `"Hello"s` creates a `std::string` directly (requires `using namespace std::string_literals`).

## std::string_view (C++17)

`#include <string_view>` — read-only, non-owning view into an existing string. No allocation, no copy.

- Prefer `std::string_view` over `const std::string&` for read-only function parameters
- No implicit conversion from `string_view` → `string`; conversion must be explicit
- Full `constexpr` support

**Ownership model:**

| Type | Role | Allocates? |
|------|------|-----------|
| `std::string` | Owner | Yes |
| `std::string_view` | Viewer | No |

**Dangling view:** a `string_view` that outlives the string it was viewing. Modifying or destroying the source string invalidates the view — results in undefined behavior.

> Full coverage: [[Chapter 5 — Constants and Strings]] → std::string
