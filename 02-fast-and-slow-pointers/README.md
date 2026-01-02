# Pattern: Fast & Slow Pointers

## Overview

The Fast & Slow Pointers pattern (also known as the Hare & Tortoise algorithm) uses two pointers that traverse a sequence at different speeds.

- `slow` moves 1 step at a time
- `fast` moves 2 steps at a time

> Core idea: At any time, the fast pointer has traveled twice the distance of the slow pointer.

## Core Technique

**Mental model:**

Fast moves one extra step per iteration relative to slow. In a finite loop, that guarantees a collision.

That's it. No math required.

**Why does this work?**

Imagine a slow pointer moving 1 step at a time and a fast pointer moving 2 steps in a circular track (the cycle).  

- Think of the **distance between fast and slow**. Every time they move, the fast pointer **closes the gap by 1 step**.  
- Why? Because fast moves 2 and slow moves 1 → fast **gains** 1 step on slow each turn.

So no matter how far apart they start, the fast pointer will eventually be 1 or 2 steps behind slow in a finite loop.

- **If the fast pointer is 1 step behind the slow pointer:** The fast pointer moves 2 steps and the slow pointer moves 1 step, and they both meet.
- **If the fast pointer is 2 steps behind the slow pointer:** The fast pointer moves 2 steps and the slow pointer moves 1 step. After the moves, the fast pointer will be 1 step behind the slow pointer, which reduces this scenario to the first scenario. This means that the two pointers will meet in the next iteration.

**Example 1: Linear linked list without cycle**

```plaintext
A ──▶ B ──▶ C ──▶ D ──▶ E ──▶ F ──▶ G ──▶ null

slow = A, fast = A
Iteration 1: slow = B, fast = C
Iteration 2: slow = C, fast = E
Iteration 3: slow = D, fast = G
Iteration 4: slow = E, fast = null (fast reached the end of the list, no cycle detected)
```

**Example 2: Linear linked list with cycle**

```plaintext
A ──▶ B ──▶ C ──▶ D ──▶ E ──▶ F
                  ▲           │
                  └───────────┘

slow = A, fast = A
Iteration 1: slow = B, fast = C
Iteration 2: slow = C, fast = E
Iteration 3: slow = D, fast = D (fast caught up with slow, cycle detected)
```

## Decision Guide

**Use when:**

- Detecting cycles
- Finding the middle of a sequence or linked list
- Reorder or split linked list (finding the middle node is often crucial)
- Identifying cycle length or cycle start
- Do all of this in $O(n)$ time and $O(1)$ space

## Problems

|  #  | Problem                                                            | Difficulty |
| :-: | :----------------------------------------------------------------: | :--------: |
| 01  | [Linked List Cycle](./01-linked-list-cycle.md)                     | 🟢 Easy    |
| 02  | [Middle of the Linked List](./02-middle-of-the-linked-list.md)     | 🟢 Easy    |
| 03  | [Start of Linked List Cycle](./03-start-of-linked-list-cycle.md)   | 🟡 Medium  |
| 04  | [Happy Number](./04-happy-number.md)                               | 🟢 Easy    |
| 05  | [Palindrome Linked List](./05-palindrome-linked-list.md)           | 🟢 Easy    |
| 06  | [Rearrange a Linked List](./06-rearrange-a-linked-list.md)         | 🟡 Medium  |
| 07  | [Cycle in a Circular Array](./07-cycle-in-a-circular-array.md)     | 🔴 Hard    |
