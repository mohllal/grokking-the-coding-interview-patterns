---
title: Reverse Nodes in k-Group
difficulty: 🔴 Hard
tags:
  - Linked List
  - Recursion
url: https://leetcode.com/problems/reverse-nodes-in-k-group/
---

# Reverse Nodes in k-Group

## Problem Description

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return the modified list.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

## Examples

**Example 1:**

```plaintext
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

**Example 2:**

```plaintext
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

## Constraints

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

## Solution

### Intuition

This problem applies the [sub-list reversal technique](./02-reverse-a-sub-list.md) repeatedly for each group of `k` nodes.

For each group of `k` nodes, we reverse it and connect it to the previous group's tail. The key insight is that after reversing a group, the original first node becomes the tail—we save this to connect the next reversed group.

```plaintext
Original:   1 ────────▶ 2 ────────▶ 3 ────────▶ 4 ────────▶ 5  (k=2)
            └──group 1──┘           └──group 2──┘

After:      2 ──▶ 1 ──▶ 4 ──▶ 3 ──▶ 5
                  │     ▲
                  └─────┘ (prev_group_tail connects to next group's new head)
```

### Algorithm

1. For each potential group, scan ahead to check if `k` nodes exist
2. If fewer than `k` nodes remain, break (they stay unchanged)
3. Reverse the group: start `prev` at `after_group`, reverse `k` nodes
4. Connect: if not the first group, link `prev_group_tail.next` to `prev` (new head of reversed group)
5. Update `prev_group_tail` to the original start (now the tail)
6. Move to the next group starting at `after_group`

### Complexity Analysis

- **Time Complexity:** $O(n)$ - each node is visited twice (once for counting, once for reversing)
- **Space Complexity:** $O(1)$ - only using a constant number of pointers

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        if head is None or k == 1:
            return head

        current_head = head
        prev_group_tail = None
        new_head = head
        while True:
            # Check if there are k nodes available
            scan = current_head
            count = 0
            while scan is not None and count < k:
                scan = scan.next
                count += 1

            if count < k:
                break  # remaining nodes stay as-is

            after_group = scan          # node after the current kth group
            group_start = current_head  # first node in the current kth group

            # Reverse the current group
            prev = after_group
            curr = group_start
            for _ in range(k):
                next_node = curr.next
                curr.next = prev
    
                prev = curr
                curr = next_node
            
            # Connect with the previous group
            if prev_group_tail is not None:
                prev_group_tail.next = prev
            else:
                new_head = prev  # first reversed group sets new head

            prev_group_tail = group_start
            current_head = after_group

        return new_head
```
