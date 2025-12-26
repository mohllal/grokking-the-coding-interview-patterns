# Two Pointers

## Overview

The **Two Pointers** technique uses two pointers (or indices) to traverse a data structure simultaneously. Instead of using nested loops ($O(n^2)$), two pointers can often solve problems in a single pass ($O(n)$).

## Common Variants

### 1. Opposite Direction (Converging)

Pointers start at opposite ends and move toward each other.

```plaintext
Array: [1, 2, 3, 4, 5, 6, 7]
        ↑                 ↑
       left             right
        →       ←
```

**Use when:**

- Array is sorted
- Finding pairs with a certain criteria

### 2. Same Direction (Fast & Slow)

Both pointers start at the same end, one moves faster or conditionally.

```plaintext
Array: [1, 1, 2, 2, 3, 4, 4]
        ↑  ↑
       slow fast
             →→
```

**Use when:**

- Removing duplicates in-place
- Partitioning elements by condition

### 3. Sliding Window Anchored

One pointer anchors, the other explores a range.

```plaintext
Array: [-2, 0, 1, 3]      target = 2
         ↑
         i (anchor)
            ↑     ↑
          left  right
```

**Use when:**

- Finding triplets/quadruplets with certain criteria

## When to Use Two Pointers

| Signal                               | Example                                     |
|--------------------------------------|---------------------------------------------|
| **Sorted array** + find pair/triplet | Two Sum II, 3Sum                            |
| **In-place** modification required   | Remove Duplicates, Remove Element           |
| **Comparing from both ends**         | Valid Palindrome, Container With Most Water |
| **Partitioning** elements            | Dutch National Flag, Sort Colors            |
| **Merging** sorted arrays            | Merge Sorted Array, Squares of Sorted Array |

## Problems

|  #  | Problem                                                                                        | Difficulty |
| :-: | :--------------------------------------------------------------------------------------------- | :--------: |
| 01  | [Two Sum II](./01-pair-with-target-sum.md)                                                     | 🟡 Medium  |
| 02  | [Find Non-Duplicate Number Instances](./02-find-non-duplicate-number-instances.md)             | 🟢 Easy    |
| 03  | [Squaring a Sorted Array](./03-squaring-a-sorted-array.md)                                     | 🟢 Easy    |
| 04  | [Triplet Sum to Zero](./04-triplet-sum-to-zero.md)                                             | 🟡 Medium  |
| 05  | [Triplet Sum Close to Target](./05-triplet-sum-close-to-target.md)                             | 🟡 Medium  |
| 06  | [Triplets with Smaller Sum](./06-triplets-with-smaller-sum.md)                                 | 🟡 Medium  |
| 07  | [Dutch National Flag Problem](./07-dutch-national-flag-problem.md)                             | 🟡 Medium  |
| 08  | [Quadruple Sum to Target](./08-quadruple-sum-to-target.md)                                     | 🟡 Medium  |
| 09  | [Backspace String Compare](./09-comparing-strings-containing-backspaces.md)                    | 🟢 Easy    |
| 10  | [Minimum Window Sort](./10-minimum-window-sort.md)                                             | 🟡 Medium  |
