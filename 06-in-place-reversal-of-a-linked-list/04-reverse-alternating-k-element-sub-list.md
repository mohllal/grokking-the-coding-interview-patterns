---
title: Reverse Alternate K Nodes in a Singly Linked List
difficulty: 🟡 Medium
tags:
  - Linked List
url: https://www.geeksforgeeks.org/reverse-alternate-k-nodes-in-a-singly-linked-list/
---

# Reverse Alternate K Nodes in a Singly Linked List

## Problem Description

Given the head of a linked list and a number `k`, reverse every alternating `k` sized sub-list starting from the head.

If, in the end, you are left with a sub-list with less than `k` elements, reverse it too.

## Examples

**Example 1:**

```plaintext
Input: head = [1,2,3,4,5,6,7,8], k = 2
Output: [2,1,3,4,6,5,7,8]

Groups:  [1,2] [3,4] [5,6] [7,8]
         ^^^^^ skip  ^^^^^ skip
        reverse     reverse
```

## Constraints

- The number of nodes in the list is `n`.
- `1 <= k <= n`

## Solution

### Intuition

This is a variation of [Reverse Nodes in k-Group](./03-reverse-every-k-element-sub-list.md). Instead of reversing every group, we alternate: reverse the first group, skip the second, reverse the third, and so on.

The key difference is tracking a `should_reverse` flag that toggles after processing each group. When skipping, we still need to traverse `k` nodes and update `prev_group_tail` to maintain proper connections.

```plaintext
Original:   1 ──────▶ 2 ──────▶ 3 ──────▶ 4 ──────▶ 5 ──────▶ 6 ──────▶ 7 ──────▶ 8  (k=2)
            └─group 1─┘         └─group 2─┘         └─group 3─┘         └─group 4─┘
             reverse               skip               reverse              skip

After:      2 ──────▶ 1 ──────▶ 3 ──────▶ 4 ──────▶ 6 ──────▶ 5 ──────▶ 7 ──────▶ 8
```

### Algorithm

1. Initialize `should_reverse = True` to reverse the first group
2. For each group, scan ahead to count available nodes
3. If `should_reverse`:
   - Reverse the group using the standard technique
   - Connect to previous group's tail
4. If skipping:
   - Just traverse `k` nodes without reversing
   - Update `prev_group_tail` to the last node of the skipped group
5. Toggle `should_reverse` and move to the next group

### Complexity Analysis

- **Time Complexity:** $O(n)$ - each node is visited at most twice
- **Space Complexity:** $O(1)$ - only using a constant number of pointers

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseAltKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        if head is None or k == 1:
            return head

        current_head = head
        prev_group_tail = None
        new_head = head
        should_reverse = True
        while current_head is not None:
            # Count available nodes in this group
            scan = current_head
            count = 0
            while scan is not None and count < k:
                scan = scan.next
                count += 1

            after_group = scan
            group_start = current_head

            if should_reverse:
                # Reverse the current group
                prev = after_group
                curr = group_start
                for _ in range(count):
                    next_node = curr.next
                    curr.next = prev

                    prev = curr
                    curr = next_node

                # Connect with the previous group
                if prev_group_tail is not None:
                    prev_group_tail.next = prev
                else:
                    new_head = prev

                prev_group_tail = group_start
            else:
                # Skip this group (don't reverse)
                # Traverse to find the last node of this group
                last_of_group = group_start
                for _ in range(count - 1):
                    last_of_group = last_of_group.next

                prev_group_tail = last_of_group

            should_reverse = not should_reverse
            current_head = after_group

        return new_head
```
