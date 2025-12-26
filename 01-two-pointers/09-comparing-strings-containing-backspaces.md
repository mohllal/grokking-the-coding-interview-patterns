---
title: Backspace String Compare
difficulty: 🟢 Easy
tags:
  - Two Pointers
  - String
  - Stack
  - Simulation
url: https://leetcode.com/problems/backspace-string-compare/
---

# Backspace String Compare

## Problem Description

Given two strings `s` and `t`, return `true` if they are equal when both are typed into empty text editors. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

## Examples

**Example 1:**

```plaintext
Input: s = "ab#c", t = "ad#c"
Output: true
Explanation: Both s and t become "ac".
```

**Example 2:**

```plaintext
Input: s = "ab##", t = "c#d#"
Output: true
Explanation: Both s and t become "".
```

**Example 3:**

```plaintext
Input: s = "a#c", t = "b"
Output: false
Explanation: s becomes "c" while t becomes "b".
```

## Constraints

- `1 <= s.length, t.length <= 200`
- `s` and `t` only contain lowercase letters and `'#'` characters.

**Follow up:** Can you solve it in `O(n)` time and `O(1)` space?

## Solution

### Intuition

Instead of building new strings (which uses O(n) space), traverse both strings **from the end**. This way, we know exactly how many characters to skip when we encounter backspaces.

### Algorithm

1. Start two pointers at the end of each string
2. For each string, find the next valid character by:
   - Count consecutive `#` characters
   - Skip that many non-`#` characters
3. Compare the valid characters from both strings
4. If they differ, or one string has remaining characters while the other doesn't, return `false`
5. Move both pointers left and repeat

### Complexity Analysis

- **Time Complexity:** $O(n + m)$ — Each character is visited at most twice.
- **Space Complexity:** $O(1)$ — Only pointers and counters.

```python
class Solution:
    def getNextValidIndex(self, string: str, index: int) -> int:
        backspaces = 0
        while index >= 0:
            if string[index] == "#":
                backspaces += 1
            elif backspaces > 0:
                backspaces -= 1
            else:
                break
            index -= 1

        return index

    def backspaceCompare(self, s: str, t: str) -> bool:
        s_index = len(s) - 1
        t_index = len(t) - 1

        while s_index >= 0 or t_index >= 0:
            s_index = self.getNextValidIndex(s, s_index)
            t_index = self.getNextValidIndex(t, t_index)
        
            # Reached the end of both the strings
            if s_index < 0 and t_index < 0:
                return True

            # Reached the end of one of the strings
            if s_index < 0 or t_index < 0:
                return False
            
            # Non-matching characters
            if s[s_index] != t[t_index]:
                return False
            
            s_index -= 1
            t_index -= 1
        
        return True
```
