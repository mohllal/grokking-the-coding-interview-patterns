---
title: Valid Parentheses
difficulty: 🟢 Easy
tags:
  - String
  - Stack
url: https://leetcode.com/problems/valid-parentheses/
---

# Valid Parentheses

## Problem Description

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

## Examples

**Example 1:**

```plaintext
Input: s = "()"
Output: true
```

**Example 2:**

```plaintext
Input: s = "()[]{}"
Output: true
```

**Example 3:**

```plaintext
Input: s = "(]"
Output: false
```

## Constraints

- `1 <= s.length <= 10⁴`
- `s` consists of parentheses only `'()[]{}'`

## Solution

### Intuition

A stack naturally models the nesting structure of brackets. Push opening brackets onto the stack; when encountering a closing bracket, check if the top of the stack has the matching opener.

### Algorithm

1. Create a mapping of closing → opening brackets
2. For each character:
   - If opening: push onto stack
   - If closing: pop and verify it matches
3. Return true if stack is empty at the end

### Complexity Analysis

- **Time Complexity:** $O(n)$ — single pass through the string
- **Space Complexity:** $O(n)$ — stack may hold all characters in worst case

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        matching = {')': '(', '}': '{', ']': '['}

        for bracket in s:
            if bracket in matching.values():
                stack.append(bracket)
            else:
                if not stack:
                    return False  # Closing bracket with nothing to match

                if stack.pop() != matching[bracket]:
                    return False  # Mismatched bracket types

        return not stack  # Valid only if all openers were closed
```
