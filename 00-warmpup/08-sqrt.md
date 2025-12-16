---
title: Sqrt(x)
difficulty: 🟢 Easy
tags:
  - Math
  - Binary Search
url: https://leetcode.com/problems/sqrtx/
---

# Sqrt(x)

## Problem Description

Given a non-negative integer `x`, return the square root of `x` rounded down to the nearest integer. The returned integer should be **non-negative** as well.

You **must not use** any built-in exponent function or operator.

- For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.

## Examples

**Example 1:**

```plaintext
Input: x = 4
Output: 2
Explanation: The square root of 4 is 2, so we return 2.
```

**Example 2:**

```plaintext
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.
```

## Constraints

- `0 <= x <= 2^31 - 1`

## Solution

The approach uses binary search to find the largest integer whose square is less than or equal to `x`. We search in the range `[1, x/2]` since for any `x >= 2`, the square root is at most `x/2`. For each midpoint, we compare `mid * mid` with `x` and adjust the search range accordingly. When the loop ends, `right` holds the floor of the square root.

### Complexity Analysis

- **Time Complexity:** $O(log n)$ - Binary search halves the search space each iteration.
- **Space Complexity:** $O(1)$ - Only constant extra space for pointers.

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        if x < 2:
            return x

        left = 1
        right = x // 2
        while left <= right:
            mid = (left + right) // 2
            number = mid * mid
            if number < x:
                left = mid + 1
            elif number > x:
                right = mid - 1
            else:
                return mid

        return right
```
