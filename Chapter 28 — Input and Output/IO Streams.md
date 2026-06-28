---
tags:
  - cpp/io
  - concept
  - syntax
aliases:
  - stream
  - input stream
  - output stream
  - iostream
  - standard stream
  - ios
  - cin
  - cout
  - cerr
  - clog
up: "[[Chapter 28 — Input and Output]]"
related:
  - "[[istream]]"
  - "[[ostream]]"
  - "[[File IO|File I/O]]"
  - "[[IO]]"
  - "[[Chapter 28 — Input and Output]]"
---

# I/O Streams

A **stream** is a sequence of bytes that can be accessed sequentially. Streams abstract the source or destination of data — a program reads from or writes to a stream without caring whether the other end is a keyboard, file, network socket, or another process.

## Stream direction

| Direction | Stream type | Operator | Purpose |
|---|---|---|---|
| Input | `istream` | `>>` (extraction) | Read values out of the stream |
| Output | `ostream` | `<<` (insertion) | Write values into the stream |
| Both | `iostream` | Both | Bidirectional I/O |

`ios` is a typedef for `std::basic_ios<char>` — the common base class that defines state flags and other shared behavior for both input and output streams.

## The four standard streams

C++ provides four pre-connected stream objects defined in `<iostream>`:

| Object | Type | Connected to | Buffering |
|---|---|---|---|
| `std::cin` | `istream` | Standard input (keyboard) | Buffered |
| `std::cout` | `ostream` | Standard output (monitor) | Buffered |
| `std::cerr` | `ostream` | Standard error (monitor) | **Unbuffered** — flushes immediately |
| `std::clog` | `ostream` | Standard error (monitor) | Buffered |

These objects are set up by the runtime before `main()` executes. They correspond to the POSIX standard file descriptors 0, 1, and 2.

For a simpler overview of `cin`/`cout`/`cerr` as used in everyday code, see [[IO]].

## Stream class hierarchy

```
ios (common base)
├── istream  — input operations (see [[istream]])
│   └── ifstream — file input (see [[File IO|File I/O]])
├── ostream  — output/formatting (see [[ostream]])
│   └── ofstream — file output (see [[File IO|File I/O]])
└── iostream — bidirectional
    └── fstream — bidirectional file I/O
```

## Subnodes

- [[istream]] — extraction operator, `setw`, `get()`, `getline()`
- [[ostream]] — formatting flags and manipulators

> Full coverage: [[Chapter 28 — Input and Output]] → I/O Streams
