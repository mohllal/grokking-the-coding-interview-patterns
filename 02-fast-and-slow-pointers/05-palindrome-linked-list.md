---
title: Palindrome Linked List
difficulty: 🟢 Easy
tags:
  - Linked List
  - Two Pointers
  - Stack
url: https://leetcode.com/problems/palindrome-linked-list/
---

# Palindrome Linked List

## Problem Description

Given the head of a singly linked list, write a method to check if the linked list is a palindrome or not.

Your algorithm should use constant space and the input linked list should be in the original form once the algorithm is finished. The algorithm should have $O(n)$ time complexity where `n` is the number of nodes in the linked list.

## Examples

**Example 1:**

```plaintext
2 ──▶ 4 ──▶ 6 ──▶ 4 ──▶ 2 ──▶ null

Input: head = [2,4,6,4,2]
Output: true
Explanation: Reads the same forwards and backwards.
```

**Example 2:**

```plaintext
2 ──▶ 4 ──▶ 6 ──▶ 4 ──▶ 2 ──▶ 2 ──▶ null

Input: head = [2,4,6,4,2,2]
Output: false
```

## Constraints

- The number of nodes in the list is in the range `[1, 10⁵]`.
- `0 <= Node.val <= 9`

## Solution 1: Using a Stack

### Intuition

A palindrome reads the same forwards and backwards. We can find the middle of the list, then push the second half onto a stack. Popping from the stack gives us the second half in reverse order, which we compare against the first half.

For odd-length lists, we skip the middle element since it doesn't affect palindrome status.

```plaintext
List: 2 ──▶ 4 ──▶ 6 ──▶ 4 ──▶ 2

First half:  [2, 4]
Middle:      6 (skip for odd length)
Second half: [4, 2] → Stack: [4, 2]
Pop order:   2, 4 → matches first half ✓
```

### Algorithm

1. Find the length and middle node using fast/slow pointers
2. Push second half values onto a stack (skip middle for odd length)
3. Compare first half with popped stack values
4. If all match, it's a palindrome

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Two passes through the list
- **Space Complexity:** $O(n)$ - Stack stores half the nodes

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def isOddLength(self, head: Optional[ListNode]) -> bool:
        fast = head
        while fast is not None and fast.next is not None:
            fast = fast.next.next

        # If the list length is EVEN: fast will eventually jump past the last node and become None.
        # If the list length is ODD: fast will land exactly on the last node (not None).
        return fast is not None

    def getMiddleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

        return slow

    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if head is None or head.next is None:
            return True

        middle = self.getMiddleNode(head)
        second_half = middle.next if self.isOddLength(head) else middle

        values_stack = []
        while second_half is not None:
            values_stack.append(second_half.val)
            second_half = second_half.next

        first_half = head
        while values_stack:
            if first_half.val != values_stack.pop():
                return False

            first_half = first_half.next

        return True
```

## Solution 2: Reverse Second Half In-Place

### Intuition

To achieve $O(1)$ space, we reverse the second half of the list in-place. Then we compare the first half with the reversed second half. Finally, we restore the list by reversing the second half again.

```plaintext
Original:     2 ──▶ 4 ──▶ 6 ──▶ 4 ──▶ 2
After split:  2 ──▶ 4 ──▶ 6    4 ──▶ 2
Reverse 2nd:  2 ──▶ 4 ──▶ 6    2 ──▶ 4
Compare:      2 == 2 ✓, 4 == 4 ✓ → palindrome
Restore:      2 ──▶ 4 ──▶ 6 ──▶ 4 ──▶ 2
```

### Algorithm

1. Find middle node using fast/slow pointers
2. Reverse the second half of the list
3. Compare first half with reversed second half
4. Reverse second half again to restore original list
5. Return result

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Multiple passes but still linear
- **Space Complexity:** $O(1)$ - Only pointers, no extra data structures

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
            next_node = current_node.next # save next node
            current_node.next = previous_node # reverse current node
    
            previous_node = current_node # update previous node
            current_node = next_node # move to next node

        return previous_node

    def getMiddleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

        return slow

    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if head is None or head.next is None:
            return True
        
        second_half = self.getMiddleNode(head)
        reversed_second_half = self.reverseList(second_half)

        p1 = head
        p2 = reversed_second_half
        result = True

        while p2 is not None:
            if p1.val != p2.val:
                result = False
                break

            p1 = p1.next
            p2 = p2.next

        self.reverseList(reversed_second_half)
        return result
```
