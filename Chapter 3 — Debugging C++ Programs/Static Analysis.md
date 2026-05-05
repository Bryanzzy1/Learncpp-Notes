---
tags:
  - cpp/debugging
  - cpp/tools
  - best-practice
  - subnode
aliases:
  - static analysis
  - clang-tidy
  - cppcheck
  - cpplint
  - linter
  - SonarLint
up: "[[Debugging]]"
related:
  - "[[Debugging]]"
  - "[[Compile-time Evaluation]]"
---

# Static Analysis Tools

Static analysis tools examine source code **without running it**, catching bugs, style violations, and non-standard patterns that the compiler itself may not flag.

| Tool | Focus |
|------|-------|
| [clang-tidy](https://clang.llvm.org/extra/clang-tidy/) | Modernization, bug-proneness, style |
| [cppcheck](https://cppcheck.sourceforge.io/) | Undefined behavior, memory errors, logic bugs |
| [cpplint](https://github.com/cpplint/cpplint) | Google C++ Style Guide compliance |
| [SonarLint](https://www.sonarsource.com/open-source-editions/) | Code quality and security vulnerabilities |

**Best practice:** run at least one static analysis tool on every C++ project. Many issues they find (uninitialized variables, null dereferences, resource leaks) would otherwise only surface at runtime.

> Full coverage: [[Chapter 3 — Debugging C++ Programs]] → Static Analysis Tools
