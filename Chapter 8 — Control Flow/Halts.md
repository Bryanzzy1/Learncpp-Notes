---
tags:
  - cpp/control-flow
  - concept
  - syntax
  - best-practice
aliases:
  - halt
  - std::exit
  - std::abort
  - std::terminate
  - std::atexit
  - std::quick_exit
  - normal termination
  - abnormal termination
  - status code
up: "[[Chapter 8 — Control Flow]]"
related:
  - "[[Control Flow]]"
  - "[[Functions]]"
  - "[[Memory Model]]"
  - "[[Static Local Variables]]"
  - "[[Global Variables]]"
---

# Halts

A **halt** is a flow control statement that terminates the program. C++ provides several halt functions in `<cstdlib>`.

## std::exit()

`std::exit()` terminates the program **normally** — the program exited in an expected way (regardless of whether it succeeded).

```cpp
std::exit(0); // terminate with status code 0 (success)
```

On exit:
1. Objects with **static storage duration** are destroyed (global and static-local variables — see [[Global Variables]] and [[Static Local Variables]]).
2. Miscellaneous file cleanup occurs.
3. Control returns to the OS with the passed status code.

**Local variables are not cleaned up** — `std::exit()` does not unwind the call stack.

`std::exit()` is called implicitly when `main()` returns. It can also be called explicitly to terminate early.

### std::atexit()

Register a cleanup function to be called automatically when `std::exit()` runs:

```cpp
void cleanup()
{
    std::cout << "cleanup!\n";
}

int main()
{
    std::atexit(cleanup); // register — pass the function, don't call it
    std::cout << 1 << '\n';
    std::exit(0);
    // statements below never execute
}
```

- The registered function must take no parameters and return `void`.
- Multiple functions can be registered; they are called in **reverse registration order**.
- In multi-threaded programs, `std::exit()` can crash if another thread is still accessing the static objects being cleaned up. Use `std::quick_exit()` / `std::at_quick_exit()` in that case — they skip static object cleanup.

## std::abort()

`std::abort()` terminates the program **abnormally** — the program encountered an unexpected runtime error and cannot continue. It performs no cleanup at all (not even static objects).

## std::terminate()

`std::terminate()` is called by the runtime in exceptional situations (unhandled exceptions, violated `noexcept`). By default it calls `std::abort()`. It is rarely called directly.

## Normal vs. abnormal termination

| | Normal (`std::exit`) | Abnormal (`std::abort`) |
|--|---------------------|------------------------|
| Static object cleanup | Yes | No |
| Stack unwinding | No | No |
| Signals OS | Exit code | Implementation-defined signal |

> Full coverage: [[Chapter 8 — Control Flow]] → Halts
