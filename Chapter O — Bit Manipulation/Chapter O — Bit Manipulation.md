---
tags:
  - cpp/bits
  - cpp/bitset
  - cpp/operators
  - concept
  - syntax
  - best-practice
  - chapter
aliases:
  - Bit Manipulation
  - ChO
up: LearnCPP
related:
  - "[[Bit Flags]]"
  - "[[Bitwise Operators]]"
  - "[[Bit Masks]]"
---

# Chapter O — Bit Manipulation

Pin: No
Fav: No
Created: April 13, 2026 8:45 PM
Last edited: May 6, 2026 5:07 PM

## Notes

- **[[Bit Flags]]**
    - When individual bits of an object are used as Boolean values, the bits are called **bit flags**.
        - In computing, a **flag** is a value that signals when some condition exists in a program.
        - std::bitset
            - test() allows us to query whether a bit is a 0 or 1.
            - set() allows us to turn a bit on (this will do nothing if the bit is already on).
            - reset() allows us to turn a bit off (this will do nothing if the bit is already off).
            - flip() allows us to flip a bit value from a 0 to a 1 or vice versa.
            
            ```cpp
            #include <bitset> // for std::bitset
            
            std::bitset<8> mybitset {}; // 8 bits in size means room for 8 flags
            std::bitset<8> bits{ 0b0000'0101 }; // we need 8 bits, start with bit pattern 0000 0101
                bits.set(3);   // set bit position 3 to 1 (now we have 0000 1101)
                bits.flip(4);  // flip bit 4 (now we have 0001 1101)
                bits.reset(4); // set bit 4 back to 0 (now we have 0000 1101)
            ```
            
            - size() returns the number of bits in the bitset.
            - count() returns the number of bits in the bitset that are set to `true`.
            - all() returns a Boolean indicating whether all bits are set to `true`.
            - any() returns a Boolean indicating whether any bits are set to `true`.
            - none() returns a Boolean indicating whether no bits are set to `true`.
- **[[Bitwise Operators]]**
    - When interpreted as an integer, the number of bits in the result of a bitwise NOT affects the value produced.
        - Leading ones do contribute value to the interpreted integer
    - Bitwise operators will promote operands with narrower integral types to `int` or `unsigned int`.
        - `operator~` and `operator<<` are width-sensitive and may produce different results depending on the width of the operand.
        - `static_cast` the result of such bitwise operations back to the narrower integral type before using to ensure correct results.
- **[[Bit Masks]]**
    - A **bit mask** is a predefined set of bits that is used to select which specific bits will be modified by subsequent operations.
