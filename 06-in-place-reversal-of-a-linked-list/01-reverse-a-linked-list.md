---
title: Reverse Linked List
difficulty: 🟢 Easy
tags:
  - Linked List
  - Recursion
url: https://leetcode.com/problems/reverse-linked-list/
---

# Reverse Linked List

## Problem Description

Given the `head` of a singly linked list, reverse the list, and return the reversed list.

## Examples

**Example 1:**

```plaintext
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**

```plaintext
Input: head = [1,2]
Output: [2,1]
```

**Example 3:**

```plaintext
Input: head = []
Output: []
```

## Constraints

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

## Solution

### Intuition

To reverse a linked list in-place, we need to flip each node's `next` pointer to point backward instead of forward.

The key insight is that we can do this in a single pass by maintaining three pointers: one for the current node, one for the previous node (which becomes the new "next"), and one to save the original next before we overwrite it.

```plaintext
Original:  1 ──▶ 2 ──▶ 3 ──▶ null

Step 1:    null ◀── 1     2 ──▶ 3 ──▶ null
Step 2:    null ◀── 1 ◀── 2     3 ──▶ null
Step 3:    null ◀── 1 ◀── 2 ◀── 3     null

Result:    3 ──▶ 2 ──▶ 1 ──▶ null
```

### Algorithm

1. Initialize `previous` to `null` and `current` to `head`
2. While `current` is not null:
   - Save `current.next` in a temp variable
   - Point `current.next` to `previous` (reverse the link)
   - Move `previous` to `current`
   - Move `current` to the saved next
3. Return `previous` (the new head)

### Complexity Analysis

- **Time Complexity:** $O(n)$ - single pass through the list
- **Space Complexity:** $O(1)$ - only using three pointers

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        previous_node = None
        current_node = head
        
        while current_node is not None:
            next_node = current_node.next
            
            current_node.next = previous_node
            previous_node = current_node

            current_node = next_node
        
        return previous_node
```
