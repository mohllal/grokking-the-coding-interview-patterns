---
title: Valid Anagram
difficulty: 🟢 Easy
tags:
  - Hash Table
  - String
  - Sorting
url: https://leetcode.com/problems/valid-anagram/
---

# Valid Anagram

## Problem Description

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

## Examples

**Example 1:**

```plaintext
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```plaintext
Input: s = "rat", t = "car"
Output: false
```

## Constraints

- `1 <= s.length, t.length <= 5 * 10^4`
- `s` and `t` consist of lowercase English letters.

**Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

## Solution

The approach first checks if the strings have equal length (a necessary condition for anagrams). Then it uses a hash map to count character frequencies in both strings. Finally, it verifies that every character appears the same number of times in both strings by comparing the counts bidirectionally.

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Linear pass to count characters and compare counts.
- **Space Complexity:** $O(1)$ - The counter is bounded by 26 lowercase letters.

```python
from collections import Counter

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        s_counter = Counter(s)
        t_counter = Counter(t)

        for letter, count in t_counter.items():
            if s_counter[letter] != count:
                return False
        
        for letter, count in s_counter.items():
            if t_counter[letter] != count:
                return False

        return True
```
