---
title: Middle of the Linked List
difficulty: 🟢 Easy
tags:
  - Linked List
  - Two Pointers
url: https://leetcode.com/problems/middle-of-the-linked-list/
---

# Middle of the Linked List

## Problem Description

Given the `head` of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return **the second middle** node.

## Examples

**Example 1:**

```plaintext
1 ──▶ 2 ──▶ [3] ──▶ 4 ──▶ 5 ──▶ null
             ▲
             │
           middle

Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.
```

**Example 2:**

```plaintext
1 ──▶ 2 ──▶ 3 ──▶ [4] ──▶ 5 ──▶ 6 ──▶ null
                   ▲
                   │
                 middle

Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
```

## Constraints

- The number of nodes in the list is in the range `[1, 100]`.
- `1 <= Node.val <= 100`

## Solution

### Intuition

Since fast moves twice as fast as slow, when fast reaches the end, slow has traveled exactly half the distance—placing it at the middle. For even-length lists, the loop condition `fast.next is not None` ensures slow lands on the second middle node.

### Algorithm

1. Initialize slow and fast pointers at head
2. Move slow one step, fast two steps each iteration
3. Stop when fast reaches the end (null or no next node)
4. Return slow—it's at the middle

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Single pass through the list
- **Space Complexity:** $O(1)$ - Only two pointers used

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast = head
        slow = head

        while fast is not None and fast.next is not None:
            fast = fast.next.next
            slow = slow.next

        return slow
```
