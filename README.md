# LearnCPP Notes

C++ notes following the [learncpp.com](https://www.learncpp.com) curriculum, structured for use in [Obsidian](https://obsidian.md).

Each chapter is a folder containing a chapter overview note and individual concept notes with frontmatter tags, aliases, and wikilinks connecting related concepts across chapters.

## Using in Obsidian

Download or clone this vault and open the `learncpp/` folder in Obsidian. Graph view and backlinks work out of the box — the notes are designed so every concept links to related concepts from other chapters.

## How these notes are built

Each chapter starts as a raw export from Notion (a `.zip` file). The conversion is handled by a Claude Code skill at `.claude/commands/new-learncpp-chapter.md`.

Drop a new chapter zip into `learncpp/`, then run `/new-learncpp-chapter` in Claude Code. The skill:

- Extracts and cleans the Notion export
- Splits the flat note into individual concept notes with proper frontmatter
- Decides a node/subnode hierarchy for the chapter's topics
- Wires cross-chapter wikilinks to existing notes from prior chapters
- Deletes the original flat file

## Chapters

| Folder | Topic |
|--------|-------|
| Chapter 1 — C++ Basics | Variables, I/O, operators |
| Chapter 2 — Functions and Files | Functions, headers, preprocessor |
| Chapter 3 — Debugging C++ Programs | Debugging strategies |
| Chapter 4 — Fundamental Data Types | Integers, floats, chars, bools |
| Chapter 5 — Constants and Strings | const, constexpr, string basics |
| Chapter 6 — Operators | Arithmetic, bitwise, relational operators |
| Chapter 7 — Scope, duration, and linkage | Scope, static duration, namespaces |
| Chapter 8 — Control Flow | if/else, switch, loops, breaks |
| Chapter 9 — Error handling | Assertions, exceptions |
| Chapter 10 — Type conversion | Implicit and explicit conversions, casts |
| Chapter 11 — Functions | Overloading, templates, default args |
| Chapter 12 — References and Pointers | lvalue refs, pointers, pass by ref/address |
| Chapter 13 — Compound Types | Enums, structs, class templates |
| Chapter 14 — Introduction to Classes | OOP basics, constructors, access control |
| Chapter 15 — More on Classes | this pointer, static members, friends, destructors |
| Chapter F — Constexpr functions | Compile-time functions |
| Chapter O — Bit Manipulation | Bitwise ops, bit flags, masks |
