---
title: First Missing Positive
difficulty: 🔴 Hard
tags:
  - Array
  - Cyclic Sort
url: https://leetcode.com/problems/first-missing-positive/
---

# First Missing Positive

## Problem Description

Given an unsorted integer array `nums`, return the smallest missing positive integer.

You must implement an algorithm that runs in $O(n)$ time and uses $O(1)$ extra space.

## Examples

**Example 1:**

```plaintext
Input: nums = [1,2,0]
Output: 3
```

**Example 2:**

```plaintext
Input: nums = [3,4,-1,1]
Output: 2
```

**Example 3:**

```plaintext
Input: nums = [7,8,9,11,12]
Output: 1
```

## Constraints

- `1 <= nums.length <= 10^5`
- `-2^31 <= nums[i] <= 2^31 - 1`

## Solution

### Intuition

If the array had all positives `1..n` present, the answer would be `n + 1`. So we only care about placing values in the range `1..n`.

Using cyclic sort, we try to place each value `x` at index `x - 1`. After placement, the first index `i` where `nums[i] != i + 1` implies `i + 1` is missing.

### Algorithm

1. For each index `i`, while `nums[i]` is in `[1..n]` and not in its correct place, swap it into index `nums[i] - 1`
2. Scan from left to right; return the first `i + 1` such that `nums[i] != i + 1`
3. If all positions match, return `n + 1`

### Complexity Analysis

- **Time Complexity:** $O(n)$ — each value is swapped into place at most once.
- **Space Complexity:** $O(1)$ — in-place.

```python
from typing import List


class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        n = len(nums)
        i = 0

        while i < n:
          # Value x belongs at index x-1
          correct_idx = nums[i] - 1

          # Rearrange the elements to place each positive integer at its correct index while
          # skipping negative numbers and numbers greater than the array size
          if nums[i] > 0 and nums[i] <= n and nums[i] != nums[correct_idx]:
            nums[i], nums[correct_idx] = nums[correct_idx], nums[i]
          else:
            i += 1

        # Find the first index where the element does not match its expected positive value
        for i, num in enumerate(nums):
          if num != i + 1:
            return i + 1

        # If all positions match, return n + 1
        return n + 1
```
