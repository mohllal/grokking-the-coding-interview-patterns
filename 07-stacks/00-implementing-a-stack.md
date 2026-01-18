---
title: Implementing a Stack
difficulty: 🟢 Easy
tags:
  - Stack
  - Design
---

# Implementing a Stack

## Problem Description

Implement a stack data structure with the following operations:

- `push(val)`: Add an element to the top
- `pop()`: Remove and return the top element
- `peek()`: Return the top element without removing it
- `size()`: Return the number of elements
- `isEmpty()`: Check if the stack is empty

## Solution 1: Array-Based Implementation

### Intuition

Use a fixed-size array with a `top` pointer tracking the index of the last valid element. Start at `-1` (empty), increment on push, decrement on pop.

### Complexity Analysis

- **Time Complexity:** $O(1)$ for all operations
- **Space Complexity:** $O(n)$ where n is the capacity of the stack

```python
class Stack:
    def __init__(self, capacity: int):
        if capacity <= 0:
            raise ValueError("Capacity must be positive")

        self.capacity = capacity
        self.arr = [None] * capacity
        self.top = -1

    def push(self, val):
        if self.top == self.capacity - 1:
            raise OverflowError("Stack overflow")

        self.top += 1
        self.arr[self.top] = val

    def pop(self):
        if self.top == -1:
            raise IndexError("Stack underflow")

        value = self.arr[self.top]
        self.arr[self.top] = None
        self.top -= 1
        return value

    def peek(self):
        if self.top == -1:
            raise IndexError("Stack is empty")

        return self.arr[self.top]

    def size(self):
        return self.top + 1

    def isEmpty(self):
        return self.top == -1

    def isFull(self):
        return self.top == self.capacity - 1
```

## Solution 2: Linked List Implementation

### Intuition

Use a singly linked list where the head is the top of the stack. Push prepends a new node, pop removes the head. No fixed capacity.

### Complexity Analysis

- **Time Complexity:** $O(1)$ for push, pop, peek, isEmpty; $O(n)$ for size
- **Space Complexity:** $O(n)$ for n elements

```python
class Node:
    def __init__(self, val, next=None):
        self.val = val
        self.next = next

class Stack:
    def __init__(self):
        self.head = None

    def push(self, val):
        self.head = Node(val, self.head)

    def pop(self):
        if self.head is None:
            raise IndexError("Stack underflow")
  
        value = self.head.val
        self.head = self.head.next
        return value

    def peek(self):
        if self.head is None:
            raise IndexError("Stack is empty")

        return self.head.val

    def size(self):
        current = self.head
        count = 0
        while current is not None:
            count += 1
            current = current.next
        return count

    def isEmpty(self):
        return self.head is None
```
