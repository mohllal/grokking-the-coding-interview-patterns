---
title: Contains Duplicate
difficulty: 🟢 Easy
tags:
  - Array
  - Hash Table
  - Sorting
url: https://leetcode.com/problems/contains-duplicate/
---

# Contains Duplicate

## Problem Description

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

## Examples

**Example 1:**

```plaintext
Input: nums = [1,2,3,1]
Output: true
Explanation: The element 1 occurs at the indices 0 and 3.
```

**Example 2:**

```plaintext
Input: nums = [1,2,3,4]
Output: false
Explanation: All elements are distinct.
```

**Example 3:**

```plaintext
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
```

## Constraints

- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`

## Solution

The solution uses a **Hash Set** to track seen elements. We iterate through the array once, checking if each number already exists in the set. If found, we immediately return `true` (duplicate detected). Otherwise, we add the number to the set and continue. If we finish the loop without finding duplicates, we return `false`.

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Single pass through the array with $O(1)$ set operations
- **Space Complexity:** $O(n)$ - Set can store up to n elements in the worst case

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        seen = set()

        for num in nums:
            if num in seen:
                return True
            seen.add(num)
       
        return False
```
