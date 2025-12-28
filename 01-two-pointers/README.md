# Pattern: Two Pointers

## Overview

The **Two Pointers** technique uses two pointers (or indices) to traverse a data structure simultaneously.

Instead of using nested loops with time complexity $O(n^2)$, two pointers can often solve problems in a single pass with time complexity $O(n)$.

> Core idea: Each pointer moves at most $n$ times.

## Common Variants

### 1. Converging Pointers (Opposite Ends)

**Mental model:**

Pointers start at opposite ends and move toward each other until they meet.

**Typical movement:**

- One pointer starts at the left
- One pointer starts at the right
- At each step, move exactly one pointer inward

**Example:**

```plaintext
Array: [1, 2, 3, 4, 5, 6, 7]
        ↑                 ↑
       left             right
        →                 ←
```

**Use when:**

- Array is sorted
- Finding pairs with a certain criteria
- Decision depends on sum / comparison
- You want to reduce the range of possible solutions

### 2. Fast–Slow Pointers (Same Direction)

**Mental model:**

Both pointers start at the same end, one moves faster (or conditionally).

> Think: "Read pointer" vs "Write pointer"

**Typical movement:**

- Both start at the same index
- `fast` always moves
- `slow` moves conditionally

**Example:**

```plaintext
Array: [1, 1, 2, 2, 3, 4, 4]
        ↑     ↑
       slow  fast
        →     →
```

**Use when:**

- In-place modification
- Removing / collapsing elements
- Partitioning based on condition
- Deduplication (removing duplicates)

👉 **For a deep dive and more focused practice problems for this pattern, see:**  
[Fast & Slow Pointers (Hare & Tortoise)](../02-fast-and-slow-pointers/README.md)

### 3. Anchored + Expanding Window (Fixed Anchor)

**Mental model:**

Fix one element → solve a two-pointer problem on the rest of the array.

> Very common in k-Sum problems

**Typical movement:**

- Outer loop fixes an anchor element
- Inner loop uses two pointers to find pairs with the anchor element
- Anchor moves → window resets

**Example:**

```plaintext
Array: [-2, 0, -1, 1, 3, 2]      target = 2
         ↑
         i (anchor)
            ↑   ↑
          left right
```

**Use when:**

- Finding triplets/quadruplets with certain criteria
- Input is sorted
- Remaining problem reduces to Two Sum

## Decision Guide

| Signal You See             | Pattern to Try First     |
| :------------------------: | :----------------------: |
| Sorted + find pair         | Converging pointers      |
| In-place modification      | Fast–Slow pointers       |
| Triplets / Quadruplets     | Anchored + Converging    |
| Compare from both ends     | Converging pointers      |
| One pass, no extra memory  | Same-direction pointers  |

## Problems

|  #  | Problem                                                                             | Difficulty |
| :-: | :---------------------------------------------------------------------------------: | :--------: |
| 01  | [Two Sum II](./01-pair-with-target-sum.md)                                          | 🟡 Medium  |
| 02  | [Find Non-Duplicate Number Instances](./02-find-non-duplicate-number-instances.md)  | 🟢 Easy    |
| 03  | [Squaring a Sorted Array](./03-squaring-a-sorted-array.md)                          | 🟢 Easy    |
| 04  | [Triplet Sum to Zero](./04-triplet-sum-to-zero.md)                                  | 🟡 Medium  |
| 05  | [Triplet Sum Close to Target](./05-triplet-sum-close-to-target.md)                  | 🟡 Medium  |
| 06  | [Triplets with Smaller Sum](./06-triplets-with-smaller-sum.md)                      | 🟡 Medium  |
| 07  | [Dutch National Flag Problem](./07-dutch-national-flag-problem.md)                  | 🟡 Medium  |
| 08  | [Quadruple Sum to Target](./08-quadruple-sum-to-target.md)                          | 🟡 Medium  |
| 09  | [Backspace String Compare](./09-comparing-strings-containing-backspaces.md)         | 🟢 Easy    |
| 10  | [Minimum Window Sort](./10-minimum-window-sort.md)                                  | 🟡 Medium  |
