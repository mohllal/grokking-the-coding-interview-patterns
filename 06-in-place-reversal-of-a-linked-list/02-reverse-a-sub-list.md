---
title: Reverse Linked List II
difficulty: 🟡 Medium
tags:
  - Linked List
url: https://leetcode.com/problems/reverse-linked-list-ii/
---

# Reverse Linked List II

## Problem Description

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return the reversed list.

## Examples

**Example 1:**

```plaintext
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

**Example 2:**

```plaintext
Input: head = [5], left = 1, right = 1
Output: [5]
```

## Constraints

- The number of nodes in the list is `n`.
- `1 <= n <= 500`
- `-500 <= Node.val <= 500`
- `1 <= left <= right <= n`

## Solution

### Intuition

This extends the basic reversal by only reversing a portion of the list. We need to:

1. Find the boundaries of the segment to reverse
2. Reverse just that segment
3. Reconnect it with the unchanged parts

The trick is to start `prev` at `after_right` (the node after the segment) so that when we finish reversing, the last node of the reversed segment already points to the rest of the list.

```plaintext
Before: 1 ─────▶ 2 ─────▶ 3 ─────▶ 4 ─────▶ 5 ─────▶ null (left=2, right=4)
        ▲        ▲                 ▲        ▲
        │        │                 │        │
        │        │                 │        │
    prev_left left_node        right_node after_right

After:  1 ─────▶ 4 ─────▶ 3 ─────▶ 2 ─────▶ 5 ─────▶ null
```

### Algorithm

1. Traverse to find `left_node` and `prev_left` (node before position `left`)
2. Traverse to find `right_node` and `after_right` (node after position `right`)
3. Reverse the segment: start with `prev = after_right`, iterate `right - left + 1` times
4. Reconnect: point `prev_left.next` to `prev` (new head of reversed segment)
5. Handle edge case: if `left == 1`, return `prev` as the new head

### Complexity Analysis

- **Time Complexity:** $O(n)$ - at most two passes through the list
- **Space Complexity:** $O(1)$ - only using a few pointers

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseBetween(self, head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]:
        if head is None or left == right:
            return head
    
        # Locate the node at position `left`
        prev_left = None
        left_node = head
        for _ in range(1, left):
            prev_left = left_node
            left_node = left_node.next

        # Locate the node at position `right`
        right_node = head
        after_right = head
        for _ in range(right):
            right_node = after_right
            after_right = right_node.next
        
        # Reverse the sublist [left, right]
        prev = after_right
        curr = left_node
        for _ in range(right - left + 1):
            next_node = curr.next
            curr.next = prev

            prev = curr
            curr = next_node

        # Reconnect the reversed sublist
        if prev_left is not None:
            prev_left.next = prev
            return head

        # left == 1: new head is the start of reversed segment
        return prev
```

---

# Similar Problem: Reverse First K Elements

## Problem Description

Given a linked list and a number `k`, reverse the first `k` elements.

```plaintext
Input: 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5, k = 3
Output: 3 ──▶ 2 ──▶ 1 ──▶ 4 ──▶ 5
```

## Solution

### Algorithm

This is simply `reverseBetween(head, 1, k)` — a direct usage of sub-list reversal.

1. Find the node after position `k`
2. Reverse the first `k` nodes, starting `prev` at `after_k`
3. Return `prev` as the new head

### Complexity Analysis

- **Time Complexity:** $O(k)$ — only traverse and reverse the first `k` nodes
- **Space Complexity:** $O(1)$ — only using a few pointers

```python
class Solution:
    def reverseFirstK(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        if head is None or k <= 1:
            return head

        # Find the node after position k
        after_k = head
        for _ in range(k):
            if after_k is None:
                break
            after_k = after_k.next

        # Reverse first k nodes
        prev = after_k
        curr = head
        for _ in range(k):
            next_node = curr.next
            curr.next = prev

            prev = curr
            curr = next_node

        return prev
```

---

# Similar Problem: Reverse Based on Size

## Problem Description

Given a linked list with `n` nodes, reverse it based on its size:

- If `n` is **even**: reverse the list in two groups of `n/2` nodes
- If `n` is **odd**: keep the middle node as-is, reverse the first `n/2` nodes and the last `n/2` nodes

```plaintext
Even (n=6): 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ 6
Output:     3 ──▶ 2 ──▶ 1 ──▶ 6 ──▶ 5 ──▶ 4

Odd (n=5):  1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5
Output:     2 ──▶ 1 ──▶ 3 ──▶ 5 ──▶ 4
                       ↑ middle stays
```

## Solution

### Algorithm

1. Count the length of the list
2. Compute `half = length // 2`
3. If even: reverse `[1, half]` and `[half+1, length]`
4. If odd: reverse `[1, half]` and `[half+2, length]`, skipping the middle node

### Complexity Analysis

- **Time Complexity:** $O(n)$ — count length once, then two sub-list reversals
- **Space Complexity:** $O(1)$ — only using a few pointers

```python
class Solution:
    def reverseBetween(self, head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]:
        if head is None or left == right:
            return head

        prev_left = None
        left_node = head
        for _ in range(1, left):
            prev_left = left_node
            left_node = left_node.next

        after_right = left_node
        for _ in range(right - left + 1):
            after_right = after_right.next

        prev = after_right
        curr = left_node
        for _ in range(right - left + 1):
            next_node = curr.next
            curr.next = prev

            prev = curr
            curr = next_node

        if prev_left is not None:
            prev_left.next = prev
            return head

        return prev

    def reverseBySize(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        # Count length
        length = 0
        curr = head
        while curr is not None:
            length += 1
            curr = curr.next

        half = length // 2

        if length % 2 == 0:
            # Even: reverse [1, half] and [half+1, length]
            head = self.reverseBetween(head, 1, half)
            head = self.reverseBetween(head, half + 1, length)
        else:
            # Odd: reverse [1, half] and [half+2, length], skip middle
            head = self.reverseBetween(head, 1, half)
            head = self.reverseBetween(head, half + 2, length)

        return head
```
