---
title: Linked List Cycle II
difficulty: 🟡 Medium
tags:
  - Hash Table
  - Linked List
  - Two Pointers
url: https://leetcode.com/problems/linked-list-cycle-ii/
---

# Linked List Cycle II

## Problem Description

Given the `head` of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

## Examples

**Example 1:**

```plaintext
3 ──▶ [2] ──▶ 0 ──▶ -4
       ▲             │
       └─────────────┘

Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

**Example 2:**

```plaintext
┌───────────┐
▼           │
[1] ──▶ 2 ──┘

Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

**Example 3:**

```plaintext
1 ──▶ null

Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

## Constraints

- The number of the nodes in the list is in the range `[0, 10^4]`.
- `-10^5 <= Node.val <= 10^5`
- `pos` is `-1` or a **valid index** in the linked-list.

## Solution 1: Using a Hash Set

### Intuition

The simplest approach: if we've seen a node before, it must be the start of the cycle. We traverse the list and store each visited node. The first node we encounter twice is where the cycle begins.

### Algorithm

1. Create a set to track visited nodes
2. Traverse the list, checking if each node exists in the set
3. If found, return it (cycle start)
4. If we reach null, no cycle exists

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Single pass through the list
- **Space Complexity:** $O(n)$ - Storing visited nodes

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        visited = set()
        current = head

        while current is not None:
            if current in visited:
                return current

            visited.add(current)
            current = current.next

        return None
```

## Solution 2: Floyd's Algorithm

### Intuition

Floyd's cycle detection gives us a meeting point inside the cycle. The key insight: **the distance from head to cycle start equals the remaining distance from meeting point to cycle start**.

**Setup and variables:**

```plaintext
head ──▶ ─ ─ ──▶ [S] ──▶ ─ ─ ──▶ [M] ──▶ ─ ─
◀────── d ──────▶ ▲  ◀──── b ─────▶         │
                  │                         │
                  └─────────────────────────┘
                  ◀────────── k ────────────▶

[S] = cycle start
[M] = meeting point
d = distance from head to cycle start [S]
b = distance from [S] to meeting point [M]
k = cycle length
```

**The math:**

When slow and fast meet at [M]:

- Slow traveled: `d + b` steps
- Fast traveled: `d + b + k` steps (completed one extra cycle)

Since fast moves twice as fast as slow:

$$2(d + b) = d + b + k$$
$$2d + 2b = d + b + k$$
$$d = k - b$$

**What does `d = k - b` mean?**

- `k - b` = remaining distance from meeting point `[M]` back to cycle start `[S]`
- `d` = distance from head to cycle start `[S]`

**These are equal!** So if we start one pointer at head and another at meeting point `[M]`, moving both one step at a time, they'll travel the same distance and meet at `[S]` (cycle start).

**Example walkthrough:**

```plaintext
List: 1 ──▶ 2 ──▶ [3] ──▶ 4 ──▶ 5 ──▶ 6
                   ▲                  │
                   └──────────────────┘

d = 2 (head to cycle start)
k = 4 (cycle length: 3 → 4 → 5 → 6 → 3)
```

**Phase 1: Find meeting point:**

| Step | Slow | Fast |
|------|------|------|
| 0    | 1    | 1    |
| 1    | 2    | 3    |
| 2    | 3    | 5    |
| 3    | 4    | 3    |
| 4    | 5    | 5 ✓  |

They meet at node `[5]`. So `b = 2` (distance from `[3]` to `[5]`).

Verify: `d = k - b` → `2 = 4 - 2` ✓

**Phase 2: Find cycle start:**

Reset one pointer to head, keep other at meeting point:

| Step | From Head | From Meeting Point |
|------|---------- | ------------------ |
| 0    | 1         | 5                  |
| 1    | 2         | 6                  |
| 2    | 3 ✓       | 3 ✓                |

After `d = 2` steps, both pointers meet at node `[3]` (cycle start).

### Algorithm

1. Use slow/fast pointers to detect cycle and find meeting point
2. Reset slow to head, keep fast at meeting point
3. Move both one step at a time
4. They meet at cycle start

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Each node visited at most twice
- **Space Complexity:** $O(1)$ - Only two pointers used

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def getCycleStart(self, head: Optional[ListNode], meetingPoint: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = meetingPoint

        while slow != fast:
            slow = slow.next
            fast = fast.next

        return slow

    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                return self.getCycleStart(head, slow)

        return None
```

## Solution 3: Using Cycle Length

### Intuition

If we know the cycle length `k`, we can find the cycle start by placing two pointers `k` steps apart. When we advance
both one step at a time from the head, the ahead pointer will complete exactly one cycle loop and meet the behind
pointer at the cycle start.

**Why does this work?**

Let `d` = distance from head to cycle start `S`. When both pointers move at the same speed:

```plaintext
head ──▶ ─ ─ ──▶ [S] ──▶ ─ ─ ──▶ ─ ─
◀───── d ───────▶ ▲                 │
                  └─────────────────┘
                  ◀─────── k ───────▶

[S] = cycle start
d = distance from head to cycle start
k = cycle length
```

- `pointer1` starts at head (position `0`)
- `pointer2` starts `k` steps ahead (position `k`)
- The gap between them is always `k` steps

When `pointer1` reaches the cycle start (after `d` steps):

- `pointer1` is at position `d` (the cycle start)
- `pointer2` is at position `d + k`

Since the cycle has length `k`, position `d + k` wraps around to position `d` within the cycle. Therefore, **they meet at the cycle start!**

**Note:** `k + d` inside the cycle is the same as `d` outside the cycle because $(k + d) \mod k = d$.

So when `pointer1` reaches `S` (position `d`), `pointer2` is also at `S` (position `d`).

**Example walkthrough:**

```plaintext
List: 1 ──▶ 2 ──▶ [3] ──▶ 4 ──▶ 5 ──▶ 6
                   ▲                  │
                   └──────────────────┘

a = 2 (nodes before cycle: 1, 2)
k = 4 (cycle length: 3 → 4 → 5 → 6 → 3)
```

**Setup:** Move `pointer2` ahead by `k = 4` steps:

```plaintext
pointer1: 1 (head)
pointer2: 1 → 2 → 3 → 4 → 5 (4 steps ahead)
```

**Move both one step at a time:**

```plaintext
Step 0: pointer1 = 1, pointer2 = 5
Step 1: pointer1 = 2, pointer2 = 6
Step 2: pointer1 = 3, pointer2 = 3 ← MEET! (6 wraps to 3)
```

After 2 steps (`a` steps), both pointers are at node 3—the cycle start!

### Algorithm

1. Detect cycle using slow/fast pointers
2. Calculate cycle length `k` by traversing the cycle from meeting point
3. Place `pointer1` at head, `pointer2` `k` steps ahead from head
4. Move both one step at a time until they meet
5. The node where they meet is the start of the cycle

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Multiple passes but still linear
- **Space Complexity:** $O(1)$ - Only pointers used

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                cycle_length = self.getCycleLength(slow)
                return self.getCycleStart(head, cycle_length)

        return None

    def getCycleLength(self, head: Optional[ListNode]) -> int:
        current = head.next
        length = 1

        while current != head:
            current = current.next
            length += 1

        return length

    def getCycleStart(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        pointer1 = head
        pointer2 = head

        for _ in range(k):
            pointer2 = pointer2.next

        while pointer1 != pointer2:
            pointer1 = pointer1.next
            pointer2 = pointer2.next

        return pointer1
```

---

# Similar Problem: [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

## Problem Description

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive. There is only one repeated number in `nums`, return this repeated number.

You must solve the problem without modifying the array `nums` and use only constant extra space.

## Examples

**Example 1:**

```plaintext
Input: [1,3,4,2,2]
Output: 2
```

**Example 2:**

```plaintext
Input: [3,1,3,4,2]
Output: 3
```

## Solution

### Intuition

Think of the array as a linked list where each value points to the next index: `index → value → index`. Since values are in range `[1, n]` and we have `n + 1` elements, there must be a duplicate, which creates a cycle.

```plaintext
Example 1: [1,3,4,2,2]

Index: 0 → 1 → 2 → 3 → 4
Value: 1   3   4   2   2

Linked list: 0 ──▶ 1 ──▶ 3 ──▶ [2] ──▶ 4
                               ▲       │
                               └───────┘
Output: 2 (cycle start)
```

**The duplicate number is the cycle's start.**

```plaintext
Example 2: [3,1,3,4,2]

Index: 0 → 1 → 2 → 3 → 4
Value: 3   1   3   4   2

Linked list: 0 ──▶ [3] ──▶ 4 ──▶ 2
                    ▲            │
                    └────────────┘
             1 (disconnected, never visited from index 0)

Output: 3 (cycle start)
```

### Algorithm

1. Use Floyd's algorithm treating `nums[i]` as the next pointer
2. Find meeting point inside cycle
3. Reset one pointer to start (index 0)
4. Move both one step → they meet at cycle start (the duplicate)

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Linear traversal
- **Space Complexity:** $O(1)$ - Only pointers used

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        slow = 0
        fast = 0

        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]

            if slow == fast:
                break

        # Find cycle start by resetting slow to head (index 0)
        # and moving both one step at a time until they meet at the cycle start.
        slow = 0
        while slow != fast:
            slow = nums[slow]
            fast = nums[fast]

        return slow
```
