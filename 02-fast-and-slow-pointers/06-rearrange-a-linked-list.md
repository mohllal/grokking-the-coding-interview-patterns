---
title: Reorder List
difficulty: 🟡 Medium
tags:
  - Linked List
  - Two Pointers
  - Stack
  - Recursion
url: https://leetcode.com/problems/reorder-list/
---

# Reorder List

## Problem Description

Given the head of a singly linked list, reorder it to: `L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...`

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

## Examples

**Example 1:**

```plaintext
1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ null

becomes

1 ──▶ 4 ──▶ 2 ──▶ 3 ──▶ null

Input: head = [1,2,3,4]
Output: [1,4,2,3]
```

**Example 2:**

```plaintext
1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ null

becomes

1 ──▶ 5 ──▶ 2 ──▶ 4 ──▶ 3 ──▶ null

Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```

## Constraints

- The number of nodes in the list is in the range `[1, 5 * 10^4]`.
- `1 <= Node.val <= 1000`

## Solution

### Intuition

This problem combines three linked list operations:

1. **Find the middle** - Split the list into two halves
2. **Reverse the second half** - So we can interleave from both ends
3. **Merge alternately** - Weave nodes from first half and reversed second half

This is similar to [Palindrome Linked List](./05-palindrome-linked-list.md) but instead of comparing, we're merging.

```plaintext
Original:     1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ null
Split:        1 ──▶ 2                  |    3 ──▶ 4 ──▶ 5 ──▶ null
Reverse 2nd:  1 ──▶ 2 ──▶ 3 ──▶ null   |    5 ──▶ 4 ──▶ 3 ──▶ null
Merge:        1 ──▶ 5 ──▶ 2 ──▶ 4 ──▶ 3 ──▶ null
```

### Algorithm

1. Find the middle node using slow/fast pointers
2. Reverse the second half of the list
3. Merge the two halves by alternating nodes

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Three linear passes (find middle, reverse, merge)
- **Space Complexity:** $O(1)$ - Only pointer manipulations, no extra storage

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def getMiddleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

        return slow

    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        previous_node = None
        current_node = head

        while current_node is not None:
            next_node = current_node.next
            current_node.next = previous_node

            previous_node = current_node
            current_node = next_node

        return previous_node

    def reorderList(self, head: Optional[ListNode]) -> None:
        if not head or not head.next:
            return head

        middle = self.getMiddleNode(head)
        second_half = self.reverseList(middle)

        first = head
        second = second_half

        while second.next is not None:
            first_next = first.next
            second_next = second.next

            first.next = second
            second.next = first_next

            first = first_next
            second = second_next
        
        return head
```
