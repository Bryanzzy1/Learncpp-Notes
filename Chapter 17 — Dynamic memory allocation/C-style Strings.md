---
tags:
  - cpp/strings
  - cpp/c-style
  - cpp/arrays
  - concept
  - syntax
  - best-practice
aliases:
  - C-style string
  - null-terminated string
  - C string
  - char array string
  - string literal
  - null terminator
up: "[[Chapter 17 — Dynamic memory allocation]]"
related:
  - "[[Strings]]"
  - "[[Pointers]]"
  - "[[Array Decay]]"
  - "[[C-style Arrays]]"
  - "[[Undefined Behavior]]"
---

# C-style Strings

A **C-style string** is a `char` array (or `const char` / `constexpr char`) where the last character is a **null terminator** (`'\0'`). The null terminator marks the end of the string, allowing functions like `strlen` to find the length without a separate size parameter.

```cpp
char str1[8] {};                   // 8 chars, all zero-initialized
const char str2[] { "string" };    // 7 chars: 's','t','r','i','n','g','\0'
constexpr char str3[] { "hello" }; // 6 chars: 'h','e','l','l','o','\0'
```

## Decay

C-style strings are C-style arrays of `char`, so they **decay** to `const char*` or `char*` in most expression contexts — see [[Array Decay]]. This allows them to be passed to C API functions that expect a `char*`.

## Buffer overflow

**Buffer overflow** (or array overflow) occurs when more data is written into a char array than it can hold. The bytes immediately beyond the array are overwritten, causing [[Undefined Behavior|undefined behavior]] and a common security vulnerability.

## strlen

`strlen(str)` returns the number of characters in the string **excluding** the null terminator. It works on a decayed `char*`, so the size of the original array is not needed — but the null terminator must be present or `strlen` will read past the end of the array.

## Symbolic constants

String literals in source code have type `const char[N]`:

```cpp
auto s1 { "Alex" };  // const char*   (decays)
auto* s2 { "Alex" }; // const char*   (explicit pointer)
auto& s3 { "Alex" }; // const char(&)[5]  (reference, no decay)
```

## Recommendations

| Situation | Prefer |
|---|---|
| Runtime mutable strings | `std::string` (from [[Strings]]) |
| Compile-time string constants | `constexpr std::string_view` |
| C-style string objects | Avoid — use `std::string` instead |
| C-style string symbolic constants | Avoid — use `constexpr std::string_view` instead |

> Full coverage: [[Chapter 17 — Dynamic memory allocation]] → C-style Strings
