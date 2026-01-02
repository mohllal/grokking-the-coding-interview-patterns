# Pattern: Two Heaps

The two heaps pattern maintains two heaps, which could be either two min heaps, two max heaps, or a min heap and a max heap.

Exploiting the heap property, the two heaps pattern is used to solve problems where we are given a set of elements such that we can divide them into two parts to find the smallest value from one part and the largest value from the other part or the smallest/largest value from both parts.

For a heap containing `N` elements, inserting or removing an element takes `O(logN)` time, while accessing the element at the root is done in `O(1)` time. The root stores the smallest element in the case of a min heap and the largest element in the case of a max heap.

## Heap Implementation

### Max Heap

```python
class MaxHeap:
    def __init__(self):
        self.heap = []

    def push(self, value):
        """
        Insert a new value into the heap.
        After insertion, the value is bubbled up to maintain the heap property.
        """
        # Add the new value to the end of the heap list
        self.heap.append(value)

        # Bubble up the newly added element to its correct position
        self._bubble_up(len(self.heap) - 1)

    def pop(self):
        """
        Remove and return the largest value (root of the heap).
        After removal, the heap property is restored by bubbling down the new root.
        """
        if self.is_empty():
            return None

        # Swap the root (largest element) with the last element
        self._swap(0, len(self.heap) - 1)

        # Remove the last element (previous root)
        max_value = self.heap.pop()

        # Bubble down the new root to restore the heap property
        self._bubble_down(0)

        return max_value

    def peek(self):
        """
        Return the largest value without removing it.
        The largest value is at the root (index 0).
        """
        if self.is_empty():
            return None

        return self.heap[0]

    def is_empty(self):
        """
        Check if the heap is empty.
        """
        return len(self.heap) == 0

    def _bubble_up(self, index):
        """
        Bubble up the element at the given index to its correct position.
        This is done by comparing the element with its parent and swapping if necessary.
        """
        while index > 0:
            parent_index = (index - 1) // 2

            # If the current element is larger than its parent, swap them and continue bubbling up
            if self.heap[index] > self.heap[parent_index]:
                self._swap(index, parent_index)
                index = parent_index  # Move up to the parent index
            else:
                break  # Stop if the heap property is satisfied

    def _bubble_down(self, index):
        """
        Bubble down the element at the given index to its correct position.
        This is done by comparing the element with its children and swapping with the larger child if necessary.
        """
        while True:
            left_child = 2 * index + 1
            right_child = 2 * index + 2
            largest = index

            # Check if the left child exists and is larger than the current element
            if left_child < len(self.heap) and self.heap[left_child] > self.heap[largest]:
                largest = left_child

            # Check if the right child exists and is larger than the current largest element
            if right_child < len(self.heap) and self.heap[right_child] > self.heap[largest]:
                largest = right_child

            # If the largest element is not the current element, swap them and continue bubbling down
            if largest != index:
                self._swap(index, largest)
                self._bubble_down(largest)
            else:
                break  # Stop if the heap property is satisfied

    def _swap(self, i, j):
        """
        Swap the elements at indices i and j in the heap.
        """
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
```

### Min Heap

```python
class MinHeap:
    def __init__(self):
        self.heap = []

    def push(self, value):
        """
        Insert a new value into the heap.
        After insertion, the value is bubbled up to maintain the heap property.
        """
        # Add the new value to the end of the heap list
        self.heap.append(value)

        # Bubble up the newly added element to its correct position
        self._bubble_up(len(self.heap) - 1)

    def pop(self):
        """
        Remove and return the smallest value (root of the heap).
        After removal, the heap property is restored by bubbling down the new root.
        """
        if self.is_empty():
            return None

        # Swap the root (smallest element) with the last element
        self._swap(0, len(self.heap) - 1)

        # Remove the last element (previous root)
        min_value = self.heap.pop()

        # Bubble down the new root to restore the heap property
        self._bubble_down(0)

        return min_value

    def peek(self):
        """
        Return the smallest value without removing it.
        The smallest value is at the root (index 0).
        """
        if self.is_empty():
            return None

        return self.heap[0]

    def is_empty(self):
        """
        Check if the heap is empty.
        """
        return len(self.heap) == 0

    def _bubble_up(self, index):
        """
        Bubble up the element at the given index to its correct position.
        This is done by comparing the element with its parent and swapping if necessary.
        """
        while index > 0:
            parent_index = (index - 1) // 2

            # If the current element is smaller than its parent, swap them and continue bubbling up
            if self.heap[index] < self.heap[parent_index]:
                self._swap(index, parent_index)
                index = parent_index  # Move up to the parent index
            else:
                break  # Stop if the heap property is satisfied

    def _bubble_down(self, index):
        """
        Bubble down the element at the given index to its correct position.
        This is done by comparing the element with its children and swapping with the smaller child if necessary.
        """
        while True:
            left_child = 2 * index + 1
            right_child = 2 * index + 2
            smallest = index

            # Check if the left child exists and is smaller than the current element
            if left_child < len(self.heap) and self.heap[left_child] < self.heap[smallest]:
                smallest = left_child

            # Check if the right child exists and is smaller than the current smallest element
            if right_child < len(self.heap) and self.heap[right_child] < self.heap[smallest]:
                smallest = right_child

            # If the smallest element is not the current element, swap them and continue bubbling down
            if smallest != index:
                self._swap(index, smallest)
                self._bubble_down(smallest)
            else:
                break

    def _swap(self, i, j):
        """
        Swap the elements at indices i and j in the heap.
        """
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
```

## Problems

|                                       Problem                                        |       Complexity        |
| :----------------------------------------------------------------------------------: | :---------------------: |
| **[Find the Median of a Number Stream](./01-find-the-median-of-a-number-stream.md)** |     :star2: :star2:     |
|              **[Sliding Window Median](./02-sliding-window-median.md)**              | :star2: :star2: :star2: |
