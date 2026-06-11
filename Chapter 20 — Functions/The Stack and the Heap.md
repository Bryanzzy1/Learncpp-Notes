---
tags:
  - cpp/memory
  - cpp/functions
  - concept
aliases:
  - call stack
  - stack
  - heap
  - stack frame
  - return address
  - stack overflow
  - memory segments
  - bss segment
  - data segment
  - code segment
up: "[[Chapter 20 — Functions]]"
related:
  - "[[Memory Model]]"
  - "[[new and delete]]"
  - "[[Memory Leaks]]"
  - "[[Local Variables]]"
  - "[[Static Local Variables]]"
  - "[[Global Variables]]"
  - "[[Functions]]"
  - "[[Recursion]]"
  - "[[Chapter 20 — Functions]]"
---

# The Stack and the Heap

A running C++ program's memory is divided into several **segments**:

| Segment | Contents |
|---|---|
| Code (text) | Compiled program instructions — read-only |
| BSS (uninitialized data) | Zero-initialized global and static variables |
| Data (initialized data) | Initialized global and static variables |
| Heap | Dynamically allocated objects |
| Call stack | Function parameters, local variables, bookkeeping |

See [[Memory Model]] for the broader picture of how C++ uses memory.

## The heap

The heap provides large, long-lived allocations via [[new and delete|`new`/`delete`]].

**Trade-offs:**

| Pro | Con |
|---|---|
| Can allocate large arrays/structs | Allocation is comparatively slow |
| Lifetime is explicit (lasts until `delete`) | Must be manually freed — risk of [[Memory Leaks]] |
| | Access requires a pointer (indirection cost) |

## The call stack

The **call stack** tracks all active [[Functions|functions]] from program start to the current execution point. It grows and shrinks with each function call and return.

### What happens on a function call

1. The program encounters a function call.
2. A **stack frame** is pushed onto the stack containing:
   - **Return address** — where execution resumes after the function exits
   - All function arguments
   - Memory for local variables
   - Saved register state
3. The CPU jumps to the function's entry point and begins executing.

### What happens on function return

1. Saved registers are restored from the stack frame.
2. The stack frame is **popped** — local variables and arguments are freed.
3. The return value is handled (via register or stack, architecture-dependent).
4. The CPU resumes at the return address.

### Stack overflow

**Stack overflow** occurs when every byte of stack memory is consumed — further frames spill into adjacent memory regions, causing undefined behavior or a crash. Common causes:

- Deep or unbounded [[Recursion]]
- Allocating large local arrays

**Trade-offs of the stack:**

| Pro | Con |
|---|---|
| Allocation is very fast | Size is limited (typically a few MB) |
| Memory is automatically freed on return | Cannot hold large arrays or structures safely |
| Size known at compile time — direct variable access | [[Local Variables|Lifetime]] tied to the enclosing scope |

> Full coverage: [[Chapter 20 — Functions]] → The Stack and the Heap
