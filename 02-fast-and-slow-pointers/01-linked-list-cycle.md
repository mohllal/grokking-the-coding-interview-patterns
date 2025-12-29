---
title: Linked List Cycle
difficulty: 🟢 Easy
tags:
  - Hash Table
  - Linked List
  - Two Pointers
url: https://leetcode.com/problems/linked-list-cycle/
---

# Linked List Cycle

## Problem Description

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

## Examples

**Example 1:**

```plaintext
3 ──▶ 2 ──▶ 0 ──▶ -4
      ▲            │
      └────────────┘

Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

**Example 2:**

```plaintext
┌─────────┐
▼         │
1 ──▶ 2 ──┘

Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the 0th node.
```

**Example 3:**

```plaintext
1 ──▶ null

Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

## Constraints

- The number of the nodes in the list is in the range `[0, 10^4]`.
- `-10^5 <= Node.val <= 10^5`
- `pos` is `-1` or a **valid index** in the linked-list.

## Solution

### Intuition

Two runners on a circular track at different speeds will eventually meet. The fast pointer moves 2 steps while slow moves 1, so fast gains 1 step per iteration. In a cycle, this gap shrinks until they collide. Without a cycle, fast reaches the end first.

### Algorithm

1. Initialize slow and fast pointers at head
2. Move slow one step, fast two steps each iteration
3. If they meet, there is a cycle
4. If fast reaches null, there is no cycle

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Each node is visited at most twice
- **Space Complexity:** $O(1)$ - Only two pointers used regardless of list size

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow = head
        fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                return True

        return False
```

---

# Similar Problem: Linked List Cycle Length

## Problem Description

Given the head of a LinkedList with a cycle, find the length of the cycle.

## Examples

**Example 1:**

```plaintext
3 ──▶ 2 ──▶ 0 ──▶ -4
      ▲            │
      └────────────┘

Input: head = [3,2,0,-4], pos = 1
Output: 2
Explanation: The cycle length is 2.
```

**Example 2:**

```plaintext
┌─────────┐
▼         │
1 ──▶ 2 ──┘

Input: head = [1,2], pos = 0
Output: 1
Explanation: The cycle length is 1.
```

**Example 3:**

```plaintext
1 ──▶ null

Input: head = [1], pos = -1
Output: 0
Explanation: There is no cycle in the linked list.
```

## Solution

### Intuition

Once we find the meeting point inside the cycle, we have a reference node within the loop. Starting from this node and walking until we return to it counts every node in the cycle exactly once.

### Algorithm

1. Use slow/fast pointers to find meeting point in cycle
2. From meeting point, traverse the cycle counting steps
3. When we return to meeting point, the count equals cycle length

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Each node is visited at most twice
- **Space Complexity:** $O(1)$ - Only two pointers used regardless of list size

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def getCycleLength(self, head: Optional[ListNode]) -> int:
        current = head.next
        length = 1

        while current != head:
            current = current.next
            length += 1

        return length

    def calculateCycleLength(self, head: Optional[ListNode]) -> int:
        slow = head
        fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                return self.getCycleLength(slow)

        return 0
```
