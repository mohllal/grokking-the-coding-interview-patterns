---
title: Missing Number
difficulty: 🟢 Easy
tags:
  - Array
  - Math
  - Bit Manipulation
  - Cyclic Sort
url: https://leetcode.com/problems/missing-number/
---

# Missing Number

## Problem Description

Given an array `nums` containing `n` **distinct** numbers in the range `[0, n]`, return the **only** number in the range that is missing from the array.

## Examples

**Example 1:**

```plaintext
Input: nums = [3,0,1]
Output: 2
```

**Example 2:**

```plaintext
Input: nums = [0,1]
Output: 2
```

**Example 3:**

```plaintext
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
```

## Constraints

- `n == nums.length`
- `1 <= n <= 10^4`
- `0 <= nums[i] <= n`
- All the numbers of `nums` are unique.

## Solution

### Intuition

This is a `0..n` variant of cyclic sort: value `x` belongs at index `x`.

We keep swapping `nums[i]` into its correct index as long as `nums[i]` is a valid index (i.e., `< n`). After placement, the first index `i` where `nums[i] != i` is the missing number; if all indices match, the missing number is `n`.

### Algorithm

1. For each index `i`, while `nums[i] < n` and `nums[i]` is not at its correct index, swap `nums[i]` with `nums[nums[i]]`
2. Scan the array; return the first index `i` where `nums[i] != i`
3. If all indices match, return `n`

### Complexity Analysis

- **Time Complexity:** $O(n)$ — each element is moved into place at most once.
- **Space Complexity:** $O(1)$ — in-place swaps.

```python
from typing import List


class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        i = 0

        while i < n:
            # Value x belongs at index x
            correct_idx = nums[i]

            # Only place values 0..n-1 (value n has no index in the array) and
            # swap if the current index does not hold the correct value
            if correct_idx < n and nums[i] != nums[correct_idx]:
                nums[i], nums[correct_idx] = nums[correct_idx], nums[i]
            else:
                i += 1

        for i, num in enumerate(nums):
            # First mismatch i != num tells us the missing number
            if num != i:
                return i

        # If all indices match, then n is missing
        return n
```
