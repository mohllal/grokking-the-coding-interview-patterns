---
title: Remove Duplicates from Sorted Array
difficulty: 🟢 Easy
tags:
  - Array
  - Two Pointers
url: https://leetcode.com/problems/remove-duplicates-from-sorted-array/
---

# Remove Duplicates from Sorted Array

## Problem Description

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates **in-place** such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**.

Consider the number of unique elements in `nums` to be `k`. After removing duplicates, return the number of unique elements `k`.

The first `k` elements of `nums` should contain the unique numbers in **sorted order**. The remaining elements beyond index `k - 1` can be ignored.

## Examples

**Example 1:**

```plaintext
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
```

**Example 2:**

```plaintext
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
```

## Constraints

- `1 <= nums.length <= 3 * 10^4`
- `-100 <= nums[i] <= 100`
- `nums` is sorted in **non-decreasing** order.

## Solution

### Intuition

Since the array is sorted, duplicates are always adjacent. We use two pointers:

- `next_non_duplicate`: tracks where to place the next unique element
- `i`: scans through the array looking for new unique values

### Algorithm

1. Initialize `next_non_duplicate = 1` (first element is always unique)
2. Iterate `i` from `0` to `n-1`:
   - If `nums[i] != nums[next_non_duplicate - 1]`:
     - Copy `nums[i]` to `nums[next_non_duplicate]`
     - Increment `next_non_duplicate`
3. Return `next_non_duplicate` as the count of unique elements

### Complexity Analysis

- **Time Complexity:** $O(n)$ — Single pass through the array.
- **Space Complexity:** $O(1)$ — In-place modification with two pointers.

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        next_non_duplicate = 1
        i = 0

        while i < len(nums):
            if nums[i] != nums[next_non_duplicate - 1]:
                nums[next_non_duplicate] = nums[i]
                next_non_duplicate += 1

            i += 1

        return next_non_duplicate
```

---

# Similar Problem: [Remove Element](https://leetcode.com/problems/remove-element/)

## Problem Description

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in-place and return the new length.

## Examples

**Example 1:**

```plaintext
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
```

**Example 2:**

```plaintext
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
```

## Solution

### Intuition

Similar to removing duplicates, but instead of comparing adjacent elements, we compare each element against the target value `val`.

### Algorithm

1. Initialize `next_non_val = 0` to track where to place non-val elements
2. Iterate through the array:
   - If `nums[i] != val`: copy it to `nums[next_non_val]` and increment
3. Return `next_non_val`

### Complexity Analysis

- **Time Complexity:** $O(n)$ — Single pass through the array.
- **Space Complexity:** $O(1)$ — In-place modification.

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        next_non_val = 0
        i = 0

        while i < len(nums):
            if nums[i] != val:
                nums[next_non_val] = nums[i]
                next_non_val += 1
            i += 1

        return next_non_val
```
