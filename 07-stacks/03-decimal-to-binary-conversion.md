---
title: Decimal to Binary Conversion
difficulty: 🟡 Medium
tags:
  - Math
  - Stack
---

# Decimal to Binary Conversion

## Problem Description

Given a positive integer `n`, write a function that returns its binary equivalent as a string. Do not use any built-in binary conversion function.

## Examples

**Example 1:**

```plaintext
Input: n = 2
Output: "10"
Explanation: The binary equivalent of 2 is 10.
```

**Example 2:**

```plaintext
Input: n = 7
Output: "111"
Explanation: The binary equivalent of 7 is 111.
```

**Example 3:**

```plaintext
Input: n = 18
Output: "10010"
Explanation: The binary equivalent of 18 is 10010.
```

## Constraints

- `1 <= n <= 10⁹`

## Solution

### Intuition

To convert decimal to binary, repeatedly divide by 2 and collect remainders. The remainders come out in reverse order (LSB first), so a stack naturally reverses them to produce the correct binary string.

### Algorithm

1. While `n != 0`:
   - Push `n % 2` onto the stack
   - Set `n = n // 2`
2. Pop all elements and join to form the binary string

### Complexity Analysis

- **Time Complexity:** $O(\log n)$ — number of bits in n
- **Space Complexity:** $O(\log n)$ — stack holds all bits

```python
class Solution: 
    def decimalToBinary(self, num: int) -> str:
        if num == 0:
            return "0"

        stack = []
        while num != 0:
            stack.append(str(num % 2))  # Remainder is the bit (LSB first)
            num //= 2
        
        return "".join(reversed(stack))  # Reverse to get MSB first
```
