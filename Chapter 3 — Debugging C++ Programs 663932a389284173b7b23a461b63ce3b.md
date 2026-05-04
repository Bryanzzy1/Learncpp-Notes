# Chapter 3 — Debugging C++ Programs

Pin: No
Fav: No
Category: Done (https://www.notion.so/Done-24fb13b569b5836bb160816e5fa04870?pvs=21)
Created: April 13, 2026 8:45 PM
Last edited: April 19, 2026 11:28 PM

## Notes

- When printing information for debugging purposes, use `std::cerr` instead of `std::cout`.
    - Unbuffered
- **Logger**
    - A **log** is a sequential record of events that have happened, usually time-stamped.
        - The process of generating a log is called **logging**.
        - Logs are written to a file on disk
    - `std::clog`
        - Better off using one of the many existing third-party logging tools available.
            - One option is plog
- **Debugger**
    - ***Stepping***
        - **Step into**
            - Executes the next statement in the normal execution path of the program, and then pauses execution of the program.
        - **Step over**
            - Executes the next statement in the normal execution path of the program.
            - *Step over* will execute an entire function without stopping and return control to you after the function has been executed.
        - **Step out**
            - Executes all remaining code in the function currently being executed, and then returns control to you when the function has returned.
    - ***Running and breakpoints***
        - **Run to cursor**
            - Executes the program until execution reaches the statement selected by your cursor.
        - **Continue**
            - Continue running the program as per normal, either until the program terminates or until a breakpoint.
        - **Start**
            - Performs the same action as *continue*, just starting from the beginning of the program.
        - **Breakpoints**
            - A special marker that tells the debugger to stop execution of the program at the breakpoint when running in debug mode.
        - **Set the next statement**
            - Allows us to change the point of execution to some other statement (sometimes informally called *jumping*).
            - Your variables will retain whatever values they had before the jump.
    - ***Watching Variables***
        - **The watch window**
            - A window where you can add variables you would like to continually inspect, and these variables will be updated as you step through your program.
            - Some debuggers will allow you to set a breakpoint on a watched variable rather than a line.
            - Some debuggers will allow you to set a breakpoint on a watched variable rather than a line.
    - ***Call stack***
        - A list of all the active functions that have been called to get to the current point of execution.
            - The line numbers after the function names show the next line to be executed in each function.
        - 
- Use a static analysis tool on your programs to help find areas where your code is non-compliant with best practices.
    - [clang-tidy](https://clang.llvm.org/extra/clang-tidy/)
    - [cpplint](https://github.com/cpplint/cpplint)
    - [cppcheck](https://cppcheck.sourceforge.io/)
    - [SonarLint](https://www.sonarsource.com/open-source-editions/)