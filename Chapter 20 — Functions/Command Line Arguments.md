---
tags:
  - cpp/functions
  - cpp/io
  - concept
  - syntax
aliases:
  - argc
  - argv
  - command line
  - command-line arguments
  - argument vectors
up: "[[Chapter 20 — Functions]]"
related:
  - "[[Functions]]"
  - "[[Strings]]"
  - "[[Chapter 20 — Functions]]"
---

# Command Line Arguments

**Command line arguments** are optional string values the operating system passes to `main()` when the program is launched.

```
WordCount Myfile.txt
```

## main() signature

To accept command line arguments, use one of these equivalent signatures:

```cpp
int main(int argc, char* argv[])
int main(int argc, char** argv)
```

- **`argc`** (*argument count*) — number of arguments, including the program name itself (always ≥ 1).
- **`argv`** (*argument vectors*) — array of C-style strings; `argv[0]` is the program name, `argv[1]` is the first user argument, and so on.

## Arguments are always strings

Even numeric arguments arrive as C-style strings. Convert with `std::stringstream`:

```cpp
#include <sstream>

std::stringstream convert{ argv[1] };
int myint{};
if (!(convert >> myint))
    myint = 0; // conversion failed — use default
```

## OS parsing caveat

The **OS parses command line arguments first**, applying its own quoting and escaping rules before passing `argc`/`argv` to the program. For example, on most systems, spaces separate arguments unless quoted:

```
WordCount "my file.txt"   // argv[1] = "my file.txt" (one argument)
WordCount my file.txt     // argv[1] = "my", argv[2] = "file.txt" (two arguments)
```

> Full coverage: [[Chapter 20 — Functions]] → Command Line Arguments
