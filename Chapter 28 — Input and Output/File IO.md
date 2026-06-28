---
tags:
  - cpp/io
  - cpp/file-io
  - concept
  - syntax
aliases:
  - file input
  - file output
  - file stream
  - ofstream
  - ifstream
  - fstream
  - file reading
  - file writing
up: "[[Chapter 28 — Input and Output]]"
related:
  - "[[IO Streams|I/O Streams]]"
  - "[[istream]]"
  - "[[ostream]]"
  - "[[Chapter 28 — Input and Output]]"
---

# File I/O

C++ file I/O uses the same stream interface as console I/O — the only difference is the class. Include `<fstream>` and open a file by constructing the stream object with a filename.

| Class | Direction | Operator |
|---|---|---|
| `std::ofstream` | Output (write) | `<<` |
| `std::ifstream` | Input (read) | `>>` |
| `std::fstream` | Both | Both |

## Writing a file

```cpp
#include <fstream>
#include <iostream>

int main()
{
    std::ofstream outf{ "Sample.txt" };

    if (!outf)
    {
        std::cerr << "Uh oh, Sample.txt could not be opened for writing!\n";
        return 1;
    }

    outf << "This is line 1\n";
    outf << "This is line 2\n";

    return 0;
    // outf's destructor closes the file when it goes out of scope
}
```

`ofstream` creates (or overwrites) the file at construction. Use the `std::ios::app` open mode to append instead of overwrite.

## Reading a file

```cpp
#include <fstream>
#include <iostream>
#include <string>

int main()
{
    std::ifstream inf{ "Sample.txt" };

    if (!inf)
    {
        std::cerr << "Uh oh, Sample.txt could not be opened for reading!\n";
        return 1;
    }

    std::string strInput{};
    while (inf >> strInput)
        std::cout << strInput << '\n';

    return 0;
    // inf's destructor closes the file when it goes out of scope
}
```

`inf >> strInput` extracts one whitespace-delimited token at a time, just like [[istream|`std::cin >>`]]. Use `std::getline(inf, line)` to read full lines (see [[getline]]).

## Key points

- Always check `if (!stream)` after construction — an unopened file stream evaluates to `false`.
- Files are closed automatically when the `ofstream`/`ifstream` destructor runs (RAII). You can also call `.close()` explicitly.
- The `[[ostream]]` formatting flags and manipulators (`setw`, `fixed`, `hex`, etc.) apply to file streams exactly as they do to `std::cout`.

> Full coverage: [[Chapter 28 — Input and Output]] → File I/O
