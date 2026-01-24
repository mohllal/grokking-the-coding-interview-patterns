---
title: Find All Numbers Disappeared in an Array
difficulty: 🟢 Easy
tags:
  - Array
  - Cyclic Sort
url: https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/
---

# Find All Numbers Disappeared in an Array

## Problem Description

Given an array `nums` of length `n` where each `nums[i]` is in the range `1..n`, some numbers appear **twice** and others appear **once**.

Return all the integers in the range `1..n` that **do not** appear in `nums`.

## Examples

**Example 1:**

```plaintext
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
```

**Example 2:**

```plaintext
Input: nums = [1,1]
Output: [2]
```

## Constraints

- `n == nums.length`
- `1 <= n <= 10^5`
- `1 <= nums[i] <= n`

## Solution

### Intuition

With cyclic sort, each value `x` belongs at index `x - 1`. We swap values into their correct positions until the current index is either correct or blocked by a duplicate.

After this placement pass, any index `i` where `nums[i] != i + 1` means the number `i + 1` never got placed, so it must be missing.

### Algorithm

1. Iterate index `i` from `0` to `n - 1`
2. Let `correct_idx = nums[i] - 1`
3. If `nums[i]` is not at `correct_idx`, swap; otherwise increment `i`
4. After placement, collect all `i + 1` where `nums[i] != i + 1`

### Complexity Analysis

- **Time Complexity:** $O(n)$ — each element is swapped into position at most once.
- **Space Complexity:** $O(1)$ extra — not counting the output list.

```python
from typing import List


class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        n = len(nums)
        i = 0

        while i < n:
            # Value x belongs at index x-1
            correct_idx = nums[i] - 1

            # Swap if the current index does not hold the correct value
            if nums[i] != nums[correct_idx]:
                nums[i], nums[correct_idx] = nums[correct_idx], nums[i]
            else:
                i += 1

        missing = []
        for i, num in enumerate(nums):
            # Any index i not holding i+1 means i+1 never appeared
            if num != i + 1:
                missing.append(i + 1)

        return missing
```
