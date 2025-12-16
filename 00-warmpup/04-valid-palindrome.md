---
title: Valid Palindrome
difficulty: 🟢 Easy
tags:
  - Two Pointers
  - String
url: https://leetcode.com/problems/valid-palindrome/
---

# Valid Palindrome

## Problem Description

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` if it is a **palindrome**, or `false` otherwise.

## Examples

**Example 1:**

```plaintext
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2:**

```plaintext
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Example 3:**

```plaintext
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

## Constraints

- `1 <= s.length <= 2 * 10^5`
- `s` consists only of printable ASCII characters.

## Solution

The approach uses two pointers starting from both ends of the string. Each pointer skips non-alphanumeric characters. When both pointers are on valid characters, we compare them (case-insensitive). If they differ, it's not a palindrome. We continue until the pointers meet.

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Each character is visited at most once.
- **Space Complexity:** $O(1)$ - Only constant extra space for pointers.

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        start = 0
        end = len(s) - 1

        while start < end:
            while start < end and not s[start].isalnum():
                start += 1

            while start < end and not s[end].isalnum():
                end -= 1

            if s[start].lower() != s[end].lower():
                return False
            
            start += 1
            end -= 1

        return True
```
