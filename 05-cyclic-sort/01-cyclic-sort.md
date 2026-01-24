---
title: Cyclic Sort
difficulty: 🟢 Easy
tags:
  - Array
  - Sorting
  - Cyclic Sort
url: https://www.geeksforgeeks.org/dsa/sort-array-contains-1-n-values-set-2/
---

# Cyclic Sort

## Problem Description

You are given an array `nums` of length `n` containing **all distinct numbers from `1..n` in arbitrary order**.

Sort the array **in-place** in $O(n)$ time using $O(1)$ extra space.

## Examples

**Example 1:**

```plaintext
Input:  [3, 1, 5, 4, 2]
Output: [1, 2, 3, 4, 5]
```

**Example 2:**

```plaintext
Input:  [2, 6, 4, 3, 1, 5]
Output: [1, 2, 3, 4, 5, 6]
```

**Example 3:**

```plaintext
Input:  [1, 5, 6, 4, 3, 2]
Output: [1, 2, 3, 4, 5, 6]
```

## Constraints

- `n == len(nums)`
- `nums` is a permutation of `1..n`

## Solution

### Intuition

Because values are in `1..n`, each value `x` belongs at index `x - 1`.

If the value at index `i` isn’t in the right place yet, we swap it into its correct position. Repeating this resolves the current index quickly because each swap places at least one number where it belongs.

### Algorithm

1. Start from `i = 0`
2. Compute `correct_idx = nums[i] - 1`
3. If `nums[i] != nums[correct_idx]`, swap them (this moves a value closer to its final place)
4. Otherwise, increment `i`
5. Continue until `i` reaches the end

### Complexity Analysis

- **Time Complexity:** $O(n)$ — each element is swapped into its final position at most once.
- **Space Complexity:** $O(1)$ — in-place swaps.

```python
from typing import List


class Solution:
    def sort(self, nums: List[int]) -> List[int]:
        n = len(nums)
        i = 0

        while i < n:
            # Value x belongs at index x-1
            correct_idx = nums[i] - 1

            # Swap if the current index does not hold the correct value
            if nums[i] != nums[correct_idx]:
                nums[i], nums[correct_idx] = nums[correct_idx], nums[i]
            else:
                # Only advance when index i is resolved
                i += 1

        return nums
```
