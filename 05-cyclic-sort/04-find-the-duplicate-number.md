---
title: Find the Duplicate Number
difficulty: 🟡 Medium
tags:
  - Array
  - Cyclic Sort
url: https://leetcode.com/problems/find-the-duplicate-number/
---

# Find the Duplicate Number

## Problem Description

Given an array `nums` containing `n + 1` integers where each integer is in the range `1..n` inclusive, there is **exactly one** repeated number. Return this repeated number.

## Examples

**Example 1:**

```plaintext
Input: nums = [1,3,4,2,2]
Output: 2
```

**Example 2:**

```plaintext
Input: nums = [3,1,3,4,2]
Output: 3
```

## Constraints

- `1 <= n <= 10^5`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- Exactly one integer is repeated (possibly more than twice).

## Solution

### Intuition

Using cyclic sort logic, value `x` belongs at index `x - 1`. While scanning, if `nums[i]` is not in the correct place, we try to swap it into its correct index.

If the correct index already contains the same value, then we’ve found the duplicate (because two indices are trying to place the same number into one slot).

### Algorithm

1. Scan the array with index `i`
2. If `nums[i] == i + 1`, the value is already in the correct position → increment `i`
3. Otherwise compute `correct_idx = nums[i] - 1`
4. If `nums[correct_idx] == nums[i]`, we’re trying to place a value into a slot that already contains it → return `nums[i]` (duplicate)
5. Else swap `nums[i]` with `nums[correct_idx]` to place `nums[i]` closer to its correct position (do not increment `i` yet)

### Complexity Analysis

- **Time Complexity:** $O(n)$ — swaps are bounded by linear moves.
- **Space Complexity:** $O(1)$ — in-place.

```python
from typing import List


class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        n = len(nums)
        i = 0

        while i < n:
            # If the number is already in its correct position, move on
            if nums[i] == i + 1:
                i += 1
                continue

            # Value x belongs at index x-1
            correct_idx = nums[i] - 1

            # If the target position already has the same value,
            # we found the duplicate
            if nums[correct_idx] == nums[i]:
                return nums[i]

            # Otherwise, place nums[i] into its correct position
            nums[i], nums[correct_idx] = nums[correct_idx], nums[i]

        return -1
```
