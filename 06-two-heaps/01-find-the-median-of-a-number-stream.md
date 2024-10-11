# Problem: Find the Median of a Number Stream

LeetCode problem: [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/).

Design a class to calculate the median of a number stream. The class should have the following two methods:

1. `insertNum(int num)`: stores the number in the class
2. `findMedian()`: returns the median of all numbers inserted in the class

If the count of numbers inserted in the class is even, the median will be the average of the middle two numbers.

## Examples

Example 1:

```plaintext
1. insertNum(3)
2. insertNum(1)
3. findMedian() -> output: 2
4. insertNum(5)
5. findMedian() -> output: 3
6. insertNum(4)
7. findMedian() -> output: 3.5
```

## Solution

Let's assume that `x` is the median of a list. This means that half of the numbers in the list will be smaller than (or equal to) `x` and the other half will be greater than (or equal to) `x`.

This leads us to an approach where we can divide the list into two halves: one half to store all the smaller numbers (let’s call it `low`) and one half to store the larger numbers (let’s call it `high`).

The median of all the numbers will either be the largest number in the `low` list or the smallest number in the `high`. If the total number of elements is even, the median will be the average of these two numbers.

The algorithm follows these steps:

1. Maintain two lists:
   1. The first half of smaller numbers (i.e., `low`) in a max-heap. We should use a max-heap as we are interested in knowing the largest number in the first half.
   2. The second half of larger numbers (i.e., `high`) in a min-heap. We should use a min-heap as we are interested in knowing the smallest number in the second half.
2. Insert a number:
    1. Add the new number to the appropriate heap based on its value (smaller numbers go to the max-heap, larger numbers to the min-heap).
    2. After insertion, balance the heaps to ensure that their sizes differ by at most one element. Max-heap should always be the list containing one more element if their size aren't the same.
3. Find the median:
    1. If the number of elements is even, the median is the average of the top elements of both heaps.
    2. If the total number of elements is odd, the median is the top element of the max-heap (i.e. `low`).

Complexity analysis:

- Time complexity: O(N * LogN)
- Space complexity: O(N)

```python
import heapq

class MedianFinder:
    def __init__(self):
        # two heaps: one for smaller half and one for larger half
        self.low = []  # max-heap (inverted to use Python's min-heap as max-heap)
        self.high = [] # min-heap

    # O(N * LogN) time
    def insertNum(self, num: int) -> None:
        # if the new number is smaller than or equal to the maximum of the lower half,
        # push it to the max-heap (low). Otherwise, push it to the min-heap (high).
        if len(self.low) == 0 or -self.low[0] >= num:
            heapq.heappush(self.low, -num)
        else:
            heapq.heappush(self.high, num)

        # balance the heaps: ensure the size difference is no more than 1
        if len(self.low) > len(self.high) + 1:
            # move the largest element of the lower half to the upper half
            top = -heapq.heappop(self.low)
            heapq.heappush(self.high, top)
        elif len(self.high) > len(self.low):
            # move the smallest element of the upper half to the lower half
            top = heapq.heappop(self.high)
            heapq.heappush(self.low, -top)

    # O(1) time
    def findMedian(self) -> float:
        if self.is_empty():
            return None

        # if even number of elements, median is the average of the roots of both heaps
        if len(self.low) == len(self.high):
            return (-self.low[0] + self.high[0]) / 2

        # if odd number of elements, median is the root of the max heap (low)
        # because max heap (low) will always have one more element than min heap (high)
        return -self.low[0]

    def is_empty(self) -> bool:
        # check if both heaps are empty (no elements have been added)
        return len(self.low) == 0 and len(self.high) == 0
```
