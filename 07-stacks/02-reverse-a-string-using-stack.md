---
title: Reverse a String Using Stack
difficulty: 🟢 Easy
tags:
  - String
  - Stack
---

# Reverse a String Using Stack

## Problem Description

Given a string, write a function that uses a stack to reverse the string. Return the reversed string.

## Examples

**Example 1:**

```plaintext
Input: "Hello, World!"
Output: "!dlroW ,olleH"
```

**Example 2:**

```plaintext
Input: "OpenAI"
Output: "IAnepO"
```

**Example 3:**

```plaintext
Input: "Stacks are fun!"
Output: "!nuf era skcatS"
```

## Constraints

- `1 <= s.length <= 10⁵`
- `s[i]` is a printable ASCII character

## Solution

### Intuition

A stack reverses order naturally: first in, last out. Push all characters onto the stack, then pop them off to build the reversed string.

### Algorithm

1. Push all characters onto the stack
2. Pop each character and append to result
3. Join and return

### Complexity Analysis

- **Time Complexity:** $O(n)$ — push and pop each character once
- **Space Complexity:** $O(n)$ — stack holds all characters

```python
class Solution:
    def reverseString(self, s: str) -> str:
        stack = list(s)  # push all characters
        reversed_list = []

        while stack:
            reversed_list.append(stack.pop())  # LIFO: last char comes out first

        return ''.join(reversed_list)
```
