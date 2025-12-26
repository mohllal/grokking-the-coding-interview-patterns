---
title: Sort Colors
difficulty: 🟡 Medium
tags:
  - Array
  - Two Pointers
  - Sorting
url: https://leetcode.com/problems/sort-colors/
---

# Sort Colors (Dutch National Flag)

## Problem Description

Given an array `nums` with `n` objects colored red, white, or blue, sort them **in-place** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

## Examples

**Example 1:**

```plaintext
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

**Example 2:**

```plaintext
Input: nums = [2,0,1]
Output: [0,1,2]
```

## Constraints

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` is either `0`, `1`, or `2`.

**Follow up:** Could you come up with a one-pass algorithm using only constant extra space?

## Solution

### Intuition

The [Dutch National Flag problem](https://en.wikipedia.org/wiki/Dutch_national_flag_problem) uses three pointers to partition the array into three sections:

- `[0, red)`: all 0s (red)
- `[red, white)`: all 1s (white)  
- `(blue, n-1]`: all 2s (blue)

The `white` pointer scans through, swapping elements to their correct sections.

### Algorithm

1. Initialize `red = 0`, `white = 0`, `blue = n - 1`
2. While `white <= blue`:
   - If `nums[white] == 0`: swap with `red`, increment both `red` and `white`
   - If `nums[white] == 2`: swap with `blue`, decrement `blue` (don't increment `white` — swapped element needs checking)
   - If `nums[white] == 1`: just increment `white`

### Complexity Analysis

- **Time Complexity:** $O(n)$ — Single pass through the array.
- **Space Complexity:** $O(1)$ — In-place sorting with three pointers.

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        red = 0
        white = 0
        blue = len(nums) - 1
        
        while white <= blue:
            if nums[white] == 0:
                nums[white], nums[red] = nums[red], nums[white]
                white += 1
                red += 1
            elif nums[white] == 2:
                nums[white], nums[blue] = nums[blue], nums[white]
                blue -= 1
            else:
                white += 1
```
