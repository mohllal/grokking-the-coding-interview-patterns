---
title: Shortest Unsorted Continuous Subarray
difficulty: 🟡 Medium
tags:
  - Array
  - Two Pointers
  - Stack
  - Greedy
  - Sorting
  - Monotonic Stack
url: https://leetcode.com/problems/shortest-unsorted-continuous-subarray/
---

# Shortest Unsorted Continuous Subarray

## Problem Description

Given an integer array `nums`, you need to find one **continuous subarray** such that if you only sort this subarray in non-decreasing order, then the whole array will be sorted in non-decreasing order.

Return the shortest such subarray and output its length.

## Examples

**Example 1:**

```plaintext
Input: nums = [2,6,4,8,10,9,15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
```

**Example 2:**

```plaintext
Input: nums = [1,2,3,4]
Output: 0
```

**Example 3:**

```plaintext
Input: nums = [1]
Output: 0
```

## Constraints

- `1 <= nums.length <= 10^4`
- `-10^5 <= nums[i] <= 10^5`

**Follow up:** Can you solve it in `O(n)` time complexity?

## Solution

### Intuition

Finding the first out-of-order elements from both ends gives us a candidate subarray. But this might not be enough — we need to extend the subarray to include:

- Any element **before** the subarray that's greater than the subarray's minimum
- Any element **after** the subarray that's less than the subarray's maximum

### Algorithm

1. Find `left`: first index where `nums[left] > nums[left + 1]` (from start)
2. Find `right`: first index where `nums[right] < nums[right - 1]` (from end)
3. If `left >= right`: array is already sorted, return 0
4. Find `min` and `max` values in the subarray `[left, right]`
5. Extend `left` leftward while `nums[left-1] > min`
6. Extend `right` rightward while `nums[right+1] < max`
7. Return `right - left + 1`

### Complexity Analysis

- **Time Complexity:** $O(n)$ — Multiple linear passes.
- **Space Complexity:** $O(1)$ — Only pointers and min/max values.

```python
class Solution:
    def findUnsortedSubarrayWindow(self, nums: List[int]) -> Tuple[int, int]:
        # Find first index from the left where order breaks
        left = 0
        while left < len(nums) - 1 and nums[left] <= nums[left + 1]:
            left += 1

        # Find first index from the right where order breaks
        right = len(nums) - 1
        while right > 0 and nums[right] >= nums[right - 1]:
            right -= 1

        return left, right

    def minimumInSubarray(self, nums: List[int], left: int, right: int) -> int:
        minimum = nums[left]
        for i in range(left + 1, right + 1):
            minimum = min(nums[i], minimum)
        
        return minimum

    def maximumInSubarray(self, nums: List[int], left: int, right: int) -> int:
        maximum = nums[left]
        for i in range(left + 1, right + 1):
            maximum = max(nums[i], maximum)
        
        return maximum

    def findUnsortedSubarray(self, nums: List[int]) -> int:
        left, right = self.findUnsortedSubarrayWindow(nums)

        # If the array is already sorted
        if left >= right:
            return 0

        # Find min and max inside the initial unsorted window
        minimum = self.minimumInSubarray(nums, left, right)
        maximum = self.maximumInSubarray(nums, left, right)

        # Expand left boundary:
        # If an element before the window is greater than the minimum inside it,
        # that element would move right after sorting, so it must be included.
        while left > 0 and nums[left - 1] > minimum:
            left -= 1

        # Expand right boundary:
        # If an element after the window is smaller than the maximum inside it,
        # that element would move left after sorting, so it must be included.
        while right < len(nums) - 1 and nums[right + 1] < maximum:
            right += 1

        return right - left + 1
```
