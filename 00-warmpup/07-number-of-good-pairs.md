---
title: Number of Good Pairs
difficulty: 🟢 Easy
tags:
  - Array
  - Hash Table
  - Math
  - Counting
url: https://leetcode.com/problems/number-of-good-pairs/
---

# Number of Good Pairs

## Problem Description

Given an array of integers `nums`, return the number of **good pairs**.

A pair `(i, j)` is called *good* if `nums[i] == nums[j]` and `i` < `j`.

## Examples

**Example 1:**

```plaintext
Input: nums = [1,2,3,1,1,3]
Output: 4
Explanation: There are 4 good pairs (0,3), (0,4), (3,4), (2,5) 0-indexed.
```

**Example 2:**

```plaintext
Input: nums = [1,1,1,1]
Output: 6
Explanation: Each pair in the array are good.
```

**Example 3:**

```plaintext
Input: nums = [1,2,3]
Output: 0
```

## Constraints

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

## Solution

The approach uses a hash map to track how many times each number has been seen so far. For each number, if we've seen it `k` times before, it can form `k` new good pairs with the current occurrence. We add this count to our result and increment the counter for that number.

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Single pass through the array.
- **Space Complexity:** $O(n)$ - Hash map to store counts of each unique number.

```python
class Solution:
    def numIdenticalPairs(self, nums: List[int]) -> int:
        good_pairs = 0
        counter = {}

        for num in nums:
            if num in counter:
                good_pairs += counter[num]
                counter[num] += 1
            else:
                counter[num] = 1

        return good_pairs
```
