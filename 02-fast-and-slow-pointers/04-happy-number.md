---
title: Happy Number
difficulty: 🟢 Easy
tags:
  - Hash Table
  - Math
  - Two Pointers
url: https://leetcode.com/problems/happy-number/
---

# Happy Number

## Problem Description

A number is called a happy number if, after repeatedly replacing it with the sum of the squares of its digits, it eventually reaches `1`.

All other (not-happy) numbers will never reach `1`. Instead, they will be stuck in a cycle of numbers which does not include `1`.

## Examples

**Example 1:**

```plaintext
Input: n = 23
Output: true

Steps:
2² + 3² = 4 + 9 = 13
1² + 3² = 1 + 9 = 10
1² + 0² = 1 + 0 = 1 ✓
```

**Example 2:**

```plaintext
Input: n = 12
Output: false

Steps:
1² + 2² = 5
5² = 25
2² + 5² = 29
2² + 9² = 85
8² + 5² = 89
8² + 9² = 145
1² + 4² + 5² = 42
4² + 2² = 20
2² + 0² = 4
4² = 16
1² + 6² = 37
3² + 7² = 58
5² + 8² = 89 ← cycle back to step 5
```

## Constraints

- `1 <= n <= 2³¹ - 1`

## Solution 1: Using a Hash Set

### Intuition

The digit-square-sum process always enters a cycle—either a cycle containing `1` (happy) or a cycle without `1` (unhappy). We can detect when we've seen a number before using a hash set.

### Algorithm

1. Compute sum of squares of digits
2. If result is `1`, return true
3. If result was seen before, return false (cycle detected)
4. Add result to set and repeat

### Complexity Analysis

- **Time Complexity:** $O(\log n)$ - Each digit extraction is $O(\log n)$, and `k` iterations until cycle detected
- **Space Complexity:** $O(\log n)$ - Storing visited numbers

Where `k` is the number of iterations to detect a cycle and it is bounded by a constant.

**Why k is bounded:**

- A number with `d` digits has max sum of squares = $d \times 81$ (max digit is `9` and $9^2 = 81$)
- The max input $2^{31} - 1$ has 10 digits, so max sum = $10 \times 81 = 810$ (already < 1000)
- For any number > 999: sum of squares < 1000 (e.g., 9999 = $4 \times 81 = 324$)
- Since even the largest input (810) is already < 1000, we enter the range [1, 999] immediately (after at most 1 iteration).
- Once in this range, there are only 999 possible values we can ever see
- By the pigeonhole principle: after at most 999 iterations, a value must repeat (cycle detected)
- Therefore, `k` ≤ 999 = $O(1)$, so total time is $O(\log n)$

```python
class Solution:
    def sumOfSquares(self, n: int) -> int:
        total = 0
        while n > 0:
            total += (n % 10) ** 2
            n = n // 10
        return total

    def isHappy(self, n: int) -> bool:
        visited = set()
        current = self.sumOfSquares(n)

        while current != 1:
            if current in visited:
                return False

            visited.add(current)
            current = self.sumOfSquares(current)

        return True
```

## Solution 2: Floyd's Cycle Detection

### Intuition

Since the process always leads to a cycle, we can use the fast and slow pointer technique. Instead of storing visited numbers, we detect the cycle by having two "runners" at different speeds. When they meet, we check if the meeting point is `1`.

Think of the sequence as an implicit linked list:

- Each number points to its digit-square-sum
- Unhappy numbers form a cycle not containing `1`
- Happy numbers cycle on `1` (since 1² = 1)

```plaintext
Unhappy number 12:

12 ──▶ 5 ──▶ 25 ──▶ 29 ──▶ 85 ──▶ 89 ──▶ ... ──▶ 58
                                   ▲              │
                                   └──────────────┘

Happy number 23:

23 ──▶ 13 ──▶ 10 ──▶ 1 ──┐
                     ▲   │
                     └───┘
```

### Algorithm

1. Initialize slow at `n`, fast at sum of squares of `n`
2. Move slow one step (one sum), fast two steps (two sums)
3. When they meet, check if meeting point equals `1`

### Complexity Analysis

- **Time Complexity:** $O(\log n)$ - Same reasoning as Solution 1, `k` is bounded by a constant
- **Space Complexity:** $O(1)$ - Only two pointers, no extra space used

```python
class Solution:
    def sumOfSquares(self, n: int) -> int:
        total = 0
        while n > 0:
            total += (n % 10) ** 2
            n = n // 10
        return total

    def isHappy(self, n: int) -> bool:
        slow = n
        fast = self.sumOfSquares(n)

        while fast != slow:
            slow = self.sumOfSquares(slow)
            fast = self.sumOfSquares(self.sumOfSquares(fast))

        return fast == 1
```
