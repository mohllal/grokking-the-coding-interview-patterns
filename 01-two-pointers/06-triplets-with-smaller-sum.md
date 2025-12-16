---
title: 3Sum Smaller
difficulty: 🟡 Medium
tags:
  - Array
  - Two Pointers
  - Binary Search
  - Sorting
url: https://leetcode.com/problems/3sum-smaller/
---

# 3Sum Smaller

## Problem Description

Given an array of `n` integers `nums` and an integer `target`, find the number of index triplets `i`, `j`, `k` with `0 <= i < j < k < n` that satisfy the condition `nums[i] + nums[j] + nums[k] < target`.

## Examples

**Example 1:**

```plaintext
Input: nums = [-2,0,1,3], target = 2
Output: 2
Explanation: Because there are two triplets which sums are less than 2:
[-2,0,1]
[-2,0,3]
```

**Example 2:**

```plaintext
Input: nums = [], target = 0
Output: 0
```

**Example 3:**

```plaintext
Input: nums = [0], target = 0
Output: 0
```

## Constraints

- `n == nums.length`
- `0 <= n <= 3500`
- `-100 <= nums[i] <= 100`
- `-100 <= target <= 100`

## Solution

### Intuition

Similar to 3Sum, but instead of finding exact sums, we count triplets with sums **less than** the target.

Key insight: In a sorted array, if `nums[i] + nums[left] + nums[right] < target`, then **all pairs** between `left` and `right` (with `nums[i]`) also form valid triplets, since replacing `nums[right]` with any smaller element still satisfies the condition.

### Algorithm

1. **Sort** the array
2. For each index `i`:
   - Early exit if `nums[i] >= target`
   - Call `search_pair` to count valid pairs using two pointers
   - If `current >= target`: decrement `right`
   - If `current < target`:
     - Add `(right - left)` to count (all pairs between `left` and `right` are valid)
     - Increment `left`
3. Return count

### Complexity Analysis

- **Time Complexity:** $O(n^2)$ — Sorting plus nested two-pointer traversal.
- **Space Complexity:** $O(1)$ — Only constant extra space (excluding sorting).

```python
class Solution:
    def search_pair(self, nums, target, left):
        count = 0
        right = len(nums) - 1

        while left < right:
            current = nums[left] + nums[right]

            if current >= target:
                right -= 1
            else:
                # since nums[right] >= nums[left], we can replace nums[right] by any 
                # number between left and right to get a sum less than the target
                count += (right - left)
                left += 1
        
        return count

    def threeSumSmaller(self, nums: List[int], target: int) -> int:
        nums.sort()
        count = 0

        for i in range(len(nums)):
            if nums[i] >= target:
                break
            
            count += self.search_pair(nums, target - nums[i], i + 1)
        
        return count
```

---

## Similar Problem: Return All Triplets

Write a function to return the **list** of all such triplets instead of the count.

### Solution

#### Algorithm

Instead of counting, we record all valid triplets by iterating from `right` down to `left` when a valid sum is found.

#### Complexity Analysis

- **Time Complexity:** $O(n^3)$ — In the worst case, we enumerate all triplets.
- **Space Complexity:** $O(n^3)$ — For storing all valid triplets.

Why $O(n^3)$? In the worst case, every possible combination of 3 elements forms a valid triplet. With `n` elements, we can pick 3 in roughly $\frac{n \times n \times n}{6}$ ways, which simplifies to $O(n^3)$.

```python
class Solution:
    def search_pair(self, nums, target, left, first, triplets):
        right = len(nums) - 1

        while left < right:
            current = nums[left] + nums[right]

            if current >= target:
                right -= 1
            else:
                # record all valid triplets with current left
                for k in range(right, left, -1):
                    triplets.append([first, nums[left], nums[k]])
                left += 1

    def threeSumSmaller(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        triplets = []

        for i in range(len(nums)):
            if nums[i] >= target:
                break
            
            self.search_pair(nums, target - nums[i], i + 1, nums[i], triplets)
        
        return triplets
```
