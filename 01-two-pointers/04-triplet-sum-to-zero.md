---
title: 3Sum
difficulty: 🟡 Medium
tags:
  - Array
  - Two Pointers
  - Sorting
url: https://leetcode.com/problems/3sum/
---

# 3Sum

## Problem Description

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

## Examples

**Example 1:**

```plaintext
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
```

**Example 2:**

```plaintext
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```

**Example 3:**

```plaintext
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```

## Constraints

- `3 <= nums.length <= 3000`
- `-10^5 <= nums[i] <= 10^5`

## Solution

### Intuition

Reduce the 3Sum problem to multiple 2Sum problems. For each element `X`, find pairs `(Y, Z)` where `Y + Z = -X`. Sorting enables the two-pointer technique and helps skip duplicates.

### Algorithm

1. **Sort** the array
2. For each index `i`:
   - Skip if `nums[i] == nums[i-1]` (avoid duplicate triplets)
   - Set `target = -nums[i]`
   - Use two pointers (`left`, `right`) to find pairs summing to `target`
   - When a valid pair is found:
     - Add triplet to result
     - Skip duplicate values for both pointers
3. Return all triplets

### Complexity Analysis

- **Time Complexity:** $O(n^2)$ — Sorting is $O(n \log n)$, then for each element we do a linear scan.
- **Space Complexity:** $O(n)$ — For sorting (depending on implementation) and storing results.

```python
class Solution:
    def searchPair(self, nums: List[int], target: int, left: int, triplets: List[List[int]]) -> None:
        right = len(nums) - 1

        while left < right:
            current = nums[left] + nums[right]

            if current == target:
                triplets.append([-target, nums[left], nums[right]])

                # Skip duplicate values for left pointer
                left += 1
                while left < right and nums[left] == nums[left - 1]:
                    left += 1
                
                # Skip duplicate values for right pointer
                right -= 1
                while left < right and nums[right] == nums[right + 1]:
                    right -= 1
            elif current < target:
                left += 1
            else:
                right -= 1
    
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()

        triplets = []
        for i in range(len(nums)):
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            
            self.searchPair(nums, -nums[i], i + 1, triplets)

        return triplets
```
