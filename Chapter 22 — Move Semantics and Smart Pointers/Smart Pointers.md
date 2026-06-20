---
tags:
  - cpp/memory
  - cpp/pointers
  - concept
  - best-practice
aliases:
  - smart pointer
  - RAII pointer
  - resource-owning wrapper
up: "[[Chapter 22 — Move Semantics and Smart Pointers]]"
related:
  - "[[RAII]]"
  - "[[new and delete]]"
  - "[[Memory Leaks]]"
  - "[[Destructors]]"
  - "[[std-unique-ptr|std::unique_ptr]]"
  - "[[std-shared-ptr|std::shared_ptr]]"
  - "[[std-weak-ptr|std::weak_ptr]]"
  - "[[Chapter 22 — Move Semantics and Smart Pointers]]"
---

# Smart Pointers

A **smart pointer** is a composition class designed to manage dynamically allocated memory and guarantee that memory is freed when the smart pointer goes out of scope. It wraps a raw pointer and ties the lifetime of the pointed-to resource to the lifetime of the wrapper object — the core [[RAII]] pattern.

## Why smart pointers exist

Manual [[new and delete]] is error-prone:
- Forgetting `delete` causes [[Memory Leaks]].
- Deleting twice causes [[Undefined Behavior]].
- Returning early or throwing an exception before `delete` leaks the resource.

A smart pointer solves all three: its [[Destructors|destructor]] calls `delete` automatically, regardless of how control leaves the scope.

## Standard smart pointers (C++11)

| Type | Ownership model |
|---|---|
| [[std-unique-ptr\|std::unique_ptr]] | Sole owner — one pointer manages the resource at a time |
| [[std-shared-ptr\|std::shared_ptr]] | Shared ownership — reference-counted; freed when the last owner is destroyed |
| [[std-weak-ptr\|std::weak_ptr]] | Non-owning observer of a shared_ptr-managed resource |

## Design rule

**Prefer smart pointers over raw `new`/`delete`.** Reserve raw pointers for non-owning observations (when you know the resource lifetime is managed elsewhere).

> Full coverage: [[Chapter 22 — Move Semantics and Smart Pointers]] → Smart Pointers
