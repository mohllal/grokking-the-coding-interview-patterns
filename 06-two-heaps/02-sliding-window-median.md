# Problem: Sliding Window Median

LeetCode Problem: [480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median/).

Given an array of numbers and a number `k`, find the median of all the `k` sized sub-arrays (or windows) of the array.

## Examples

Example 1:

```plaintext
Input: nums = [1, 2, -1, 3, 5], k = 2
Output: [1.5, 0.5, 1.0, 4.0]

Explanation: Lets consider all windows of size '2':
[1, 2, -1, 3, 5] -> median is 1.5
[1, 2, -1, 3, 5] -> median is 0.5
[1, 2, -1, 3, 5] -> median is 1.0
[1, 2, -1, 3, 5] -> median is 4.0
```

Example 2:

```plaintext
Input: nums = [1, 2, -1, 3, 5], k = 3
Output: [1.0, 2.0, 3.0]

Explanation: Lets consider all windows of size '3':
[1, 2, -1, 3, 5] -> median is 1.0
[1, 2, -1, 3, 5] -> median is 2.0
[1, 2, -1, 3, 5] -> median is 3.0
```

## Solution 1

This problem follows the two heaps pattern and share similarities with [Find the Median of a Number Stream](./01-find-the-median-of-a-number-stream.md) problem.

The only difference is that we need to keep track of a sliding window of `k` numbers. This means, in each iteration, when we insert a new number in the heaps, we need to remove one number from the heaps which is going out of the sliding window. After the removal, we need to re-balance the heaps in the same way that we did while inserting.

Complexity analysis:

- Time complexity: O(N * K)
- Space complexity: O(N)

```python
import heapq

# O(LogK) time
def balanceHeaps(low: List[int], high: List[int]):
    if len(low) > len(high) + 1:
        # move the largest element of the lower half to the higher half
        top = -heapq.heappop(low)
        heapq.heappush(high, top)
    elif len(high) > len(low):
        # move the smallest element of the higher half to the lower half
        top = heapq.heappop(high)
        heapq.heappush(low, -top)

# O(LogK) time
def insertNumber(low: List[int], high: List[int], num: int):
    # if the new number is smaller than or equal to the maximum of the lower half,
    # push it to the max-heap (low). Otherwise, push it to the min-heap (high).
    if len(low) == 0 or -low[0] >= num:
        heapq.heappush(low, -num)
    else:
        heapq.heappush(high, num)
    
    # balance the heaps: ensure the size difference is no more than 1
    balanceHeaps(low, high)

# O(K) time
def removeNumber(low: List[int], high: List[int], num: int):
    for i in range(len(low)):
        if -low[i] == num:
            low[len(low) - 1], low[i] = low[i], low[len(low) - 1]
            low.pop()
            heapq.heapify(low)
            break
    else:
        for i in range(len(high)):
            if high[i] == num:
                high[len(high) - 1], high[i] = high[i], high[len(high) - 1]
                high.pop()
                heapq.heapify(high)
                break
    
    # balance the heaps: ensure the size difference is no more than 1       
    balanceHeaps(low, high)

# O(1) time
def findMedian(low: List[int], high: List[int]) -> Optional[float]:
    if len(low) == 0 and len(high) == 0:
        return None
    
    # if even number of elements, median is the average of the roots of both heaps
    if len(low) == len(high):
        return (-low[0] + high[0]) / 2
    
    # if odd number of elements, median is the root of the max heap (low)
    # because max heap (low) will always have one more element than min heap (high)
    return -low[0]

def medianSlidingWindow(nums: List[int], k: int) -> List[float]:
    low = []
    high = []
    medians = []

    window_start = 0
    for window_end in range(len(nums)):
        insertNumber(low, high, nums[window_end])

        if window_end >= k - 1:
            median = findMedian(low, high)
            medians.append(median)

            removeNumber(low, high, nums[window_start])
            window_start += 1
    
    return medians
```

## Solution 2

This algorithm is an optimized version of the previous one, instead of immediately removing the outgoing element from the heap, it simply marks the element for future removal.

During subsequent operations, the algorithm checks and removes any elements that are marked in the `to_remove` dictionary when accessing the top of the heaps. This approach significantly optimizes the time complexity, reducing the removal operation from `O(K)` to `O(LogK)` amortized time per operation.

Complexity analysis:

- Time complexity: O(N * LogK)
- Space complexity: O(N)

```python
import heapq
from collections import defaultdict

# O(1) time
def findMedian(low: List[int], high: List[int], k: int) -> Optional[float]:
    if len(low) == 0 and len(high) == 0:
        return None
    
    if k % 2:
        # if k is odd, the median is the top of low heap
        return float(-low[0])
    else:
        # if k is even, the median is the average of the tops of both heaps
        return (-low[0] + high[0]) / 2.0

def medianSlidingWindow(nums: List[int], k: int) -> List[float]:
    if len(nums) == 0 or k == 0:
        return []

    low = [] # max-heap to store the lower half of numbers (invert values for Python's min-heap)
    high = [] # min-heap to store the upper half of numbers
    to_remove = defaultdict(int) # tracks elements that need to be removed from heaps
    medians = []

    # initialize the heaps with the first k elements
    for i in range(k):
        heappush(low, -nums[i])
        heappush(high, -heappop(low))

        # balance the heaps if high has more elements
        if len(high) > len(low):
            heappush(low, -heappop(high))

    # calculate and store the median for the first window
    median = findMedian(low, high, k)
    medians.append(median)

    # iterate through the array starting from the k-th element
    for i in range(k, len(nums)):
        # mark the number that is sliding out of the window for removal
        out_num = nums[i - k]
        to_remove[out_num] += 1

        # determine which heap the outgoing number belongs to by comparing with the current median
        # if the outgoing number is less than or equal to the median, it was part of the low (max-heap)
        # otherwise, it was part of the high (min-heap)
        balance = -1 if out_num <= median else 1
    
        # add the new incoming number to the appropriate heap based on its value relative to the median
        if nums[i] <= median:
            balance += 1
            heappush(low, -nums[i])
        else:
            balance -= 1
            heappush(high, nums[i])
    
        # re-balance the heaps to maintain size properties after addition
        if balance < 0:
            # if high heap has more elements, move the smallest from high to low to balance
            # when we remove an element from low (max-heap) and then add a new one to high (min-heap)
            heappush(low, -heappop(high))
        elif balance > 0:
            # if low heap has more elements, move the largest from low to high to balance
            # when we remove an element from high (min-heap) and then add a new one to low (max-heap)
            heappush(high, -heappop(low))

        # remove elements from low heap that are marked for removal
        while low and to_remove[-low[0]] > 0:
            to_remove[-low[0]] -= 1
            heappop(low)
        
        # remove elements from high heap that are marked for removal
        while high and to_remove[high[0]] > 0:
            to_remove[high[0]] -= 1
            heappop(high)
        
        # calculate and store the current median after re-balancing and removals
        median = findMedian(low, high, k)
        medians.append(median)
    
    return medians
```
