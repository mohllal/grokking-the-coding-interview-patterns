---
title: Set Mismatch
difficulty: 🟢 Easy
tags:
  - Array
  - Cyclic Sort
url: https://leetcode.com/problems/set-mismatch/
---

# Find the Corrupt Pair

## Problem Description

You have a set of integers `s`, which originally contains all the numbers from `1` to `n`. Unfortunately, due to some error, one of the numbers in `s` got duplicated to another number in the set, which results in repetition of one number and loss of another number.

You are given an integer array `nums` representing the data status of this set after the error.

Find the number that occurs twice and the number that is missing and return them in the form of an array.

## Examples

**Example 1:**

```plaintext
Input: nums = [3,1,2,5,2]
Output: [2,4]
```

**Example 2:**

```plaintext
Input: nums = [3,1,2,3,6,4]
Output: [3,5]
```

## Constraints

- `n == nums.length`
- `2 <= nums.length <= 10^4`
- `1 <= nums[i] <= 10^4`

## Solution

### Intuition

We use cyclic sort to place each value `x` at index `x - 1`. Since one value is duplicated, one index can’t be filled correctly.

After placement, the index `i` where `nums[i] != i + 1` gives both answers:

- `duplicate = nums[i]` (the value that ended up in the wrong spot)
- `missing = i + 1` (the value that never got placed)

### Algorithm

1. Run cyclic sort swaps placing `x` at index `x - 1` when possible
2. Scan for the first index `i` where `nums[i] != i + 1`
3. Return `[nums[i], i + 1]`

### Complexity Analysis

- **Time Complexity:** $O(n)$ — bounded swaps and one scan.
- **Space Complexity:** $O(1)$ extra — in-place.

```python
from typing import List


class Solution:
    def findCorruptPair(self, nums: List[int]) -> List[int]:
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

        for i, num in enumerate(nums):
            # First mismatch reveals: duplicate = num, missing = i+1
            if num != i + 1:
                return [num, i + 1]

        return [-1, -1]
```
