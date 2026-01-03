# Pattern: In-Place Reversal of a Linked List

## Overview

The In-Place Reversal pattern manipulates linked list node pointers directly to reverse connections without allocating extra memory. Instead of creating new nodes or using auxiliary data structures, it rewires existing `next` pointers.

> Core idea: At each step, redirect the current node's `next` pointer to the previous node, then advance forward.

## Core Technique

**Mental model:**

Three pointers working in tandem: `previous`, `current`, and `next`. Save what's ahead, reverse the current link, then march forward.

**The reversal loop:**

```plaintext
while current is not null:
    next = current.next       # 1. Save the next node
    current.next = previous   # 2. Reverse the pointer

    previous = current        # 3. Move previous forward
    current = next            # 4. Move current forward
```

**Why does this work?**

Each iteration processes one node:

- Before we lose access to the rest of the list, we save `current.next`
- Then we can safely point `current.next` backward
- After processing, `previous` holds the last reversed node (new head when done)

**Example: Reversing a full list**

```plaintext
Initial:   1 ────▶ 2 ────▶ 3 ────▶ null
           ▲
           │
       current=1, previous=null

Step 1:    null ◀──── 1     2 ─────▶ 3 ─────▶ null
                      ▲     ▲
                      │     │
                  previous current

Step 2:    null ◀──── 1 ◀──── 2     3 ─────▶ null
                              ▲     ▲
                              │     │
                          previous current

Step 3:    null ◀──── 1 ◀──── 2 ◀──── 3      null
                                      ▲       ▲
                                      │       │
                                previous   current

Result:    3 ────▶ 2 ────▶ 1 ────▶ null (previous is new head)
```

## Decision Guide

**Use when:**

- Reversing all or part of a linked list
- Reordering nodes without extra space
- Problems mention "in-place" or $O(1)$ space constraint
- Rotating or shifting linked list segments

**Common patterns to recognize:**

- "Reverse from position m to n"
- "Reverse every k nodes"
- "Rotate list by k positions" (rotation = cut + reverse + reconnect)

## Problems

|  #  | Problem                                                                                  | Difficulty |
| :-: | :--------------------------------------------------------------------------------------: | :--------: |
| 01  | [Reverse a Linked List](./01-reverse-a-linked-list.md)                                   | 🟢 Easy    |
| 02  | [Reverse a Sub-List](./02-reverse-a-sub-list.md)                                         | 🟡 Medium  |
| 03  | [Reverse Every K-element Sub-List](./03-reverse-every-k-element-sub-list.md)             | 🔴 Hard    |
| 04  | [Reverse Alternating K-element Sub-List](./04-reverse-alternating-k-element-sub-list.md) | 🟡 Medium  |
| 05  | [Rotate a Linked List](./05-rotate-a-linked-list.md)                                     | 🟡 Medium  |
