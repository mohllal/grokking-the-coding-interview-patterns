---
title: Rotate List
difficulty: 🟡 Medium
tags:
  - Linked List
  - Two Pointers
url: https://leetcode.com/problems/rotate-list/
---

# Rotate List

## Problem Description

Given the `head` of a linked list, rotate the list to the right by `k` places.

## Examples

**Example 1:**

```plaintext
Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]

Original: 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5
                      ↑ cut here
Rotated:  4 ──▶ 5 ──▶ 1 ──▶ 2 ──▶ 3
```

**Example 2:**

```plaintext
Input: head = [0,1,2], k = 4
Output: [2,0,1]

k = 4 % 3 = 1 (effective rotation)
```

## Constraints

- The number of nodes in the list is in the range `[0, 500]`.
- `-100 <= Node.val <= 100`
- `0 <= k <= 2 * 10⁹`

## Solution

### Intuition

Rotating right by `k` means taking the last `k` nodes and moving them to the front. The key insight is that rotating by `length` returns the original list, so we only need to rotate by `k % length`.

The problem reduces to: find the cut point at position `length - k`, break the list there, and rewire the tail to point to the original head.

```plaintext
Original:   1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5,  k = 2
                       ↑           ↑
                   new_tail      tail

After:      4 ──▶ 5 ──▶ 1 ──▶ 2 ──▶ 3 ──▶ null
            ↑           ↑           ↑
         new_head   (tail.next)  new_tail.next = null
```

### Algorithm

1. Count the length and find the tail node
2. Compute effective rotation: `k % length` (if 0, return as-is)
3. Find the new tail at position `length - k` from head
4. Set `new_head = new_tail.next`
5. Rewire: `new_tail.next = null`, `tail.next = head`
6. Return `new_head`

### Complexity Analysis

- **Time Complexity:** $O(n)$ - two passes at most (count + find cut point)
- **Space Complexity:** $O(1)$ - only using a few pointers

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def rotateRight(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        if head is None or k == 0:
            return head

        # Find length and tail
        length = 1
        tail = head
        while tail.next:
            tail = tail.next
            length += 1
        
        rotations = k % length
        if rotations == 0:
            return head

        # Find new tail (node before the cut point)
        steps_to_new_head = length - rotations
        new_tail = head
        for _ in range(steps_to_new_head - 1):
            new_tail = new_tail.next
        
        new_head = new_tail.next

        # Rewire pointers
        new_tail.next = None
        tail.next = head

        return new_head
```
