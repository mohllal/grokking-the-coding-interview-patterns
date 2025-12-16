---
title: 3Sum Closest
difficulty: 🟡 Medium
tags:
  - Array
  - Two Pointers
  - Sorting
url: https://leetcode.com/problems/3sum-closest/
---

# 3Sum Closest

## Problem Description

Given an integer array `nums` of length `n` and an integer `target`, find three integers in `nums` such that the sum is closest to `target`.

Return the sum of the three integers.

You may assume that each input would have exactly one solution.

## Examples

**Example 1:**

```plaintext
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

**Example 2:**

```plaintext
Input: nums = [0,0,0], target = 1
Output: 0
Explanation: The sum that is closest to the target is 0. (0 + 0 + 0 = 0).
```

## Constraints

- `3 <= nums.length <= 500`
- `-1000 <= nums[i] <= 1000`
- `-10^4 <= target <= 10^4`

## Solution

### Intuition

Similar to 3Sum, but instead of finding exact matches, we track the closest sum. For each element, use two pointers to search for a pair that minimizes the distance to `target`.

At each step:

- If `current_sum < target`: move `left` right to increase the sum
- If `current_sum > target`: move `right` left to decrease the sum
- If `current_sum == target`: return immediately (can't get closer than 0)

When two sums have the same distance, prefer the smaller sum.

### Algorithm

1. **Sort** the array
2. Initialize `closest_sum` with infinity distance
3. For each index `i`:
   - Use two pointers (`left`, `right`) to find pairs
   - Update `closest_sum` if current sum is closer, or same distance but smaller
   - Move pointers based on whether current sum is less or greater than target
4. Return `closest_sum`

### Complexity Analysis

- **Time Complexity:** $O(n^2)$ — Sorting plus nested two-pointer search.
- **Space Complexity:** $O(1)$ — Only constant extra space (excluding sorting).

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums.sort()
        closest_sum = float('inf')

        for i in range(len(nums) - 2):
            left = i + 1
            right = len(nums) - 1

            while left < right:
                current_sum = nums[i] + nums[left] + nums[right]

                current_distance = abs(current_sum - target)
                closest_distance = abs(closest_sum - target)

                current = (current_distance, current_sum)
                closest = (closest_distance, closest_sum)

                if current < closest:
                    closest_sum = current_sum

                if current_sum == target:
                    return current_sum
                elif current_sum < target:
                    left += 1
                else:
                    right -= 1

        return closest_sum
```
