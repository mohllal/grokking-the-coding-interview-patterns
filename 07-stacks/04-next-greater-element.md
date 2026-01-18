---
title: Next Greater Element I
difficulty: 🟢 Easy
tags:
  - Array
  - Hash Table
  - Stack
  - Monotonic Stack
url: https://leetcode.com/problems/next-greater-element-i/
---

# Next Greater Element I

## Problem Description

The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return an array `ans` of length `nums1.length` such that `ans[i]` is the next greater element as described above.

## Examples

**Example 1:**

```plaintext
Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
Output: [-1,3,-1]
Explanation:
- 4 is at index 2 in nums2; no greater element to its right → -1
- 1 is at index 0 in nums2; next greater is 3 → 3
- 2 is at index 3 in nums2; no greater element to its right → -1
```

**Example 2:**

```plaintext
Input: nums1 = [2,4], nums2 = [1,2,3,4]
Output: [3,-1]
```

## Constraints

- `1 <= nums1.length <= nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 10⁴`
- All integers in `nums1` and `nums2` are unique
- All integers of `nums1` also appear in `nums2`

## Solution

### Intuition

Use a **monotonic decreasing stack** to efficiently find the next greater element for each number in `nums2`. Process from right to left: the stack maintains candidates that could be "next greater" for elements to the left.

### Algorithm

1. Initialize a hash map with all `nums2` values mapped to `-1`
2. Traverse `nums2` from right to left:
   - Pop elements from stack that are ≤ current (they can't be "next greater" for anything)
   - If stack is non-empty, the top is the next greater element
   - Push current element onto stack
3. Look up each `nums1` element in the hash map

### Complexity Analysis

- **Time Complexity:** $O(n + m)$ — each element pushed/popped at most once
- **Space Complexity:** $O(n)$ — stack and hash map for `nums2`

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        stack = []  # Monotonic decreasing stack
        next_greater = {num: -1 for num in nums2}

        for num in reversed(nums2):
            # Pop smaller elements, they can't be "next greater" for anything to the left
            while stack and stack[-1] <= num:
                stack.pop()

            if stack:
                next_greater[num] = stack[-1]  # The top of the stack is the next greater element

            stack.append(num)

        return [next_greater[num] for num in nums1]
```
