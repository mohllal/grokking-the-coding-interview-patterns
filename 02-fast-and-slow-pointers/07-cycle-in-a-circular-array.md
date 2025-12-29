---
title: Circular Array Loop
difficulty: 🟡 Medium
tags:
  - Array
  - Hash Table
  - Two Pointers
url: https://leetcode.com/problems/circular-array-loop/
---

# Circular Array Loop

## Problem Description

You are playing a game involving a circular array of non-zero integers `nums`. Each `nums[i]` denotes the number of indices forward/backward you must move if you are located at index `i`:

- If `nums[i]` is positive, move `nums[i]` steps forward
- If `nums[i]` is negative, move `nums[i]` steps backward

Since the array is circular, you may assume that moving forward from the last element puts you on the first element, and moving backwards from the first element puts you on the last element.

A **cycle** in the array consists of a sequence of indices `seq` of length `k` where:

- Following the movement rules above results in the repeating index sequence
- All `nums[seq[j]]` are either all positive or all negative (same direction)
- `k > 1` (cycle length must be greater than 1)

Return `true` if there is a cycle in `nums`, or `false` otherwise.

## Examples

**Example 1:**

```plaintext
Input: nums = [2, -1, 1, 2, 2]

Index: 0    1    2    3    4
Value: 2   -1    1    2    2

Movement: 0 ──▶ 2 ──▶ 3 ──▶ 0 (cycle!)

     0 ──▶ 2 ──▶ 3
     ▲           │
     └───────────┘

Output: true
Explanation: Cycle at indices 0 -> 2 -> 3 -> 0. All values positive.
```

**Example 2:**

```plaintext
Input: nums = [-1, -2, -3, -4, -5, 6]

Index:  0    1    2    3    4    5
Value: -1   -2   -3   -4   -5    6

Movement from 0: 0 ──▶ 4 ──▶ 4 (self-loop, invalid)
Movement from 5: 5 ──▶ 5 (self-loop, invalid)

Output: false
Explanation: No valid cycle. Self-loops don't count.
```

**Example 3:**

```plaintext
Input: nums = [1, -1, 5, 1, 4]

Index: 0    1    2    3    4
Value: 1   -1    5    1    4

Movement: 0 ──▶ 1 ──▶ 0 (mixed directions: +1 then -1)

Output: false
Explanation: Cycle exists but has mixed directions.
```

## Constraints

- `1 <= nums.length <= 5000`
- `-1000 <= nums[i] <= 1000`
- `nums[i] != 0`

## Solution 1: Using Linked List Representation

### Intuition

Think of the array as a linked list where each index points to its next index based on the value. We can then use cycle detection similar to [Linked List Cycle](./01-linked-list-cycle.md).

However, we need additional checks:

1. Cycle must have length > 1 (no self-loops)
2. All elements in cycle must have same direction (all positive or all negative)

### Algorithm

1. Build a linked list representation of the array
2. For each starting index, run cycle detection with direction checking
3. Return true if a valid cycle is found

### Complexity Analysis

- **Time Complexity:** $O(n^2)$ - For each starting point, we may traverse the entire array
- **Space Complexity:** $O(n)$ - Storing the linked list nodes

```python
class ListNode:
    def __init__(self, value: int, direction: bool):
        self.value = value
        self.direction = direction
        self.next = None

class Solution:
    def getNextIndex(self, nums: List[int], index: int) -> int:
        n = len(nums)
        return (index + nums[index]) % n

    def hasCycleWithSameDirection(self, head: ListNode) -> bool:
        slow = head
        fast = head

        while fast is not None and fast.next is not None:
            # check for self-loops for slow and fast pointers
            if slow == slow.next or fast == fast.next or fast.next == fast.next.next:
                break

            # check for mixed directions
            if not (slow.direction == fast.direction == fast.next.direction):
                break

            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                return True

        return False

    def circularArrayLoop(self, nums: List[int]) -> bool:
        n = len(nums)
        nodes = []

        for i in range(n):
            direction = nums[i] > 0
            nodes.append(ListNode(i, direction))

        for i in range(n):
            next_index = self.getNextIndex(nums, i)
            nodes[i].next = nodes[next_index]

        for i in range(n):
            if self.hasCycleWithSameDirection(nodes[i]):
                return True

        return False
```

## Solution 2: In-Place with Marking

### Intuition

We can optimize space by detecting cycles directly on the array. After checking each starting index, we mark visited elements as `0` to avoid rechecking them.

**Key insight:** If we start from index `i` and don't find a valid cycle, then no valid cycle can start from any index we visited. Because they all follow the same path that leads to either:

- A direction change (invalid)
- A self-loop (invalid)
- Already visited nodes

So we mark them as `0` (visited) to skip in future iterations.

### Algorithm

1. For each unvisited index, run cycle detection
2. Check for self-loops and mixed directions during traversal
3. Cycle detection with validation:
   - Move slow pointer one step, fast pointer two steps
   - If either returns `-1`, the path is invalid → stop
   - If `slow == fast` and both are valid → cycle found!
4. Mark visited nodes: If no valid cycle found from this start, mark all nodes along the path as `0`
5. Early return: Return `true` immediately when a valid cycle is found

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Each element is visited at most twice (once for detection, once for marking)
- **Space Complexity:** $O(1)$ - Only modifying input array, no extra data structures

```python
class Solution:
    def getNextIndex(self, nums: List[int], original_direction: bool, current_index: int) -> int:
        """
        Returns the next index in the circular array following the game rules.
        Returns -1 if the path becomes invalid:
            1. Direction changes (mix of positive/negative values)
            2. Self-loop detected (next index equals current index)
        """
        current_direction = nums[current_index] >= 0

        # Invalid: direction changed from original
        if current_direction != original_direction:
            return -1

        # Compute next index with circular wrapping
        next_index = (current_index + nums[current_index]) % len(nums)

        # Invalid: single-element cycle (self-loop)
        if next_index == current_index:
            return -1

        return next_index

    def hasCycleFromIndex(self, nums: List[int], start: int) -> bool:
        n = len(nums)
        original_direction = nums[start] >= 0  # True = forward, False = backward

        slow, fast = start, start

        while True:
            # Move slow one step
            slow = self.getNextIndex(nums, original_direction, slow)
            
            # Move fast two steps (if first step is valid)
            fast = self.getNextIndex(nums, original_direction, fast)
            if fast != -1:
                fast = self.getNextIndex(nums, original_direction, fast)

            # Stop if: path invalid (-1) OR cycle detected (slow == fast)
            if slow == -1 or fast == -1 or slow == fast:
                break

        # Check if we found a valid cycle (both pointers valid and equal)
        if slow != -1 and slow == fast:
            return True

        # No valid cycle found - mark all nodes in this path as visited
        index = start
        while nums[index] != 0 and (nums[index] >= 0) == original_direction:
            next_index = (index + nums[index]) % n
            nums[index] = 0  # Mark as visited
            index = next_index

        return False

    def circularArrayLoop(self, nums: List[int]) -> bool:
        for i in range(len(nums)):
            # Skip already visited nodes (marked as 0)
            if nums[i] == 0:
                continue

            if self.hasCycleFromIndex(nums, i):
                return True

        return False
```
