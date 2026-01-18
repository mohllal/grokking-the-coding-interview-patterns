# Pattern: Stacks

## Overview

A stack is a Last-In-First-Out (LIFO) data structure where elements are added and removed from the same end (the "top"). This property makes stacks ideal for problems involving nested structures, reversal, or backtracking.

> Core idea: The last element pushed is the first element popped — perfect for matching pairs, undoing operations, or reversing order.

## Core Technique

**Mental model:**

Think of a stack of plates: you can only add or remove from the top.

This constraint is powerful for the following types of problems:

- **Matching pairs:** Push openers, pop when you find closers
- **Reversal:** Push in order, pop in reverse order
- **Backtracking:** Push state, pop to undo

**Basic operations:**

```plaintext
push(x)   → Add x to the top            O(1)
pop()     → Remove and return top       O(1)
peek()    → Return top without removing O(1)
isEmpty() → Check if stack is empty     O(1)
```

**Example: Matching parentheses**

```plaintext
Input: "([{}])"

Step 1: '(' → push    Stack: ['(']
Step 2: '[' → push    Stack: ['(', '[']
Step 3: '{' → push    Stack: ['(', '[', '{']
Step 4: '}' → pop '{' Stack: ['(', '[']       ✓ matches
Step 5: ']' → pop '[' Stack: ['(']            ✓ matches
Step 6: ')' → pop '(' Stack: []               ✓ matches

Result: Valid (stack empty at end)
```

## Decision Guide

**Use when:**

- Matching nested or paired elements (brackets, tags)
- Finding next/previous greater/smaller element
- Reversing sequences or operations
- Evaluating expressions (postfix, infix)
- Simulating recursion iteratively
- Path or history navigation (undo, back button)

## Problems

|  #  | Problem                                                              | Difficulty |
| :-: | :------------------------------------------------------------------: | :--------: |
| 00  | [Implementing a Stack](./00-implementing-a-stack.md)                 | 🟢 Easy    |
| 01  | [Balanced Parentheses](./01-balanced-parentheses.md)                 | 🟢 Easy    |
| 02  | [Reverse a String Using Stack](./02-reverse-a-string-using-stack.md) | 🟢 Easy    |
| 03  | [Decimal to Binary Conversion](./03-decimal-to-binary-conversion.md) | 🟡 Medium  |
| 04  | [Next Greater Element](./04-next-greater-element.md)                 | 🟢 Easy    |
| 05  | [Sort a Stack](./05-sort-a-stack.md)                                 | 🟢 Easy    |
| 06  | [Simplify Path](./06-simplify-path.md)                               | 🟡 Medium  |
