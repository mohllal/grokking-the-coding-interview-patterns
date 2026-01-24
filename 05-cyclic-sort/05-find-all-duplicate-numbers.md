---
title: Find All Duplicates in an Array
difficulty: 🟡 Medium
tags:
  - Array
  - Cyclic Sort
url: https://leetcode.com/problems/find-all-duplicates-in-an-array/
---

# Find All Duplicates in an Array

## Problem Description

Given an integer array `nums` of length `n` where each integer is in the range `1..n`, some elements appear **twice** and others appear **once**.

Return an array of all the integers that appear **twice**.

## Examples

**Example 1:**

```plaintext
Input: nums = [4,3,2,7,8,2,3,1]
Output: [2,3]
```

**Example 2:**

```plaintext
Input: nums = [1,1,2]
Output: [1]
```

**Example 3:**

```plaintext
Input: nums = [1]
Output: []
```

## Constraints

- `n == nums.length`
- `1 <= n <= 10^5`
- `1 <= nums[i] <= n`
- Each integer appears once or twice.

## Solution

### Intuition

We cyclic-sort the array so that every value `x` tries to go to index `x - 1`. Duplicates prevent perfect placement (because their correct slot is already taken).

After the placement pass, any index `i` where `nums[i] != i + 1` means `nums[i]` couldn’t be placed correctly, which can only happen if that value already exists at its correct position — i.e., it’s a duplicate.

### Algorithm

1. Run cyclic sort swaps placing `x` at index `x - 1` when possible
2. After placement, collect all values `nums[i]` where `nums[i] != i + 1`

### Complexity Analysis

- **Time Complexity:** $O(n)$ — linear number of swaps.
- **Space Complexity:** $O(1)$ extra — excluding the output list.

```python
from typing import List


class Solution:
    def findDuplicates(self, nums: List[int]) -> List[int]:
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

        duplicates = []
        for i, num in enumerate(nums):
            # Values that couldn't land in their correct slot are duplicates
            if num != i + 1:
                duplicates.append(num)

        return duplicates
```
