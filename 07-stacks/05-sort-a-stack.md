---
title: Sort a Stack
difficulty: 🟢 Easy
tags:
  - Stack
  - Sorting
---

# Sort a Stack

## Problem Description

Given a stack, sort it using only stack operations (`push` and `pop`).

You can use an additional temporary stack, but you may not copy the elements into any other data structure (such as an array). The values in the stack are to be sorted in descending order, with the largest elements on top.

## Examples

**Example 1:**

```plaintext
Input: [34, 3, 31, 98, 92, 23]
Output: [3, 23, 31, 34, 92, 98]
        (98 on top)
```

**Example 2:**

```plaintext
Input: [4, 3, 2, 10, 12, 1, 5, 6]
Output: [1, 2, 3, 4, 5, 6, 10, 12]
        (12 on top)
```

**Example 3:**

```plaintext
Input: [20, 10, -5, -1]
Output: [-5, -1, 10, 20]
        (20 on top)
```

## Solution

### Intuition

Use an auxiliary stack to build a sorted result. For each element from the input, find its correct position in the sorted stack by temporarily moving larger elements back to the input stack.

Think of it as **insertion sort** using two stacks.

### Algorithm

1. Pop an element from the input stack
2. While the sorted stack's top is greater than this element:
   - Move the top back to the input stack
3. Push the element onto the sorted stack
4. Repeat until input is empty

### Complexity Analysis

- **Time Complexity:** $O(n^2)$ — each element may cause up to n moves
- **Space Complexity:** $O(n)$ — auxiliary stack holds all elements

```python
class Solution:
    def sortStack(self, stack: List[int]) -> List[int]:
        result = []

        while stack:
            top = stack.pop()

            # Move larger elements back to input to find correct position
            while result and result[-1] > top:
                stack.append(result.pop())

            result.append(top)  # Insert in sorted position

        return result
```
