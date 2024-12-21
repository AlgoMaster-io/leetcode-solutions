# [Leetcode 295: Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Table of Contents
- [Approach 1: Sort and Calculate Median](#approach-1-sort-and-calculate-median)
- [Approach 2: Insertion and Balanced Heaps (Optimal)](#approach-2-insertion-and-balanced-heaps-optimal)

---

## Approach 1: Sort and Calculate Median

### Intuition
The simplest approach to finding the median from a data stream is to keep all numbers in a list, sort it every time you need to calculate the median, and then return the median value. Although this solution is straightforward, sorting for each insertion results in an inefficient method.

### Solution
```python
class MedianFinder:
    def __init__(self):
        # Initialize an empty list to store the numbers
        self.nums = []

    def addNum(self, num: int) -> None:
        # Add the number to the list
        self.nums.append(num)

    def findMedian(self) -> float:
        # Sort the list
        self.nums.sort()
        n = len(self.nums)
        # If the number of elements is odd, return the middle element
        if n % 2 == 1:
            return float(self.nums[n // 2])
        # If the number of elements is even, return the average of the middle two elements
        else:
            return (self.nums[n // 2] + self.nums[n // 2 - 1]) / 2.0
```

### Time Complexity
- `addNum()`: O(1), simple list insertion
- `findMedian()`: O(n log n), due to sorting the list

### Space Complexity
- `O(n)`, where `n` is the number of elements in the list

---

## Approach 2: Insertion and Balanced Heaps (Optimal)

### Intuition
To efficiently find the median, we can use two heaps:
- A max-heap to store the smaller half of the numbers
- A min-heap to store the larger half of the numbers

This way, the median is always at the top of these heaps, and we can maintain a balance between them when adding new numbers.

### Solution
```python
import heapq

class MedianFinder:
    def __init__(self):
        # Max-heap to store the smaller half of the numbers
        self.small = []  # (a max-heap, implemented as a negative min-heap)
        # Min-heap to store the larger half of the numbers
        self.large = []

    def addNum(self, num: int) -> None:
        # Add to max-heap
        if not self.small or num <= -self.small[0]:
            heapq.heappush(self.small, -num)
        else:
            heapq.heappush(self.large, num)

        # Balance the heaps so that their size difference is no larger than 1
        if len(self.small) > len(self.large) + 1:
            heapq.heappush(self.large, -heapq.heappop(self.small))
        elif len(self.large) > len(self.small):
            heapq.heappush(self.small, -heapq.heappop(self.large))

    def findMedian(self) -> float:
        # If the number of elements is odd, the median is the top of the max-heap
        if len(self.small) > len(self.large):
            return float(-self.small[0])
        # If even, it's the average of the tops of both heaps
        else:
            return (-self.small[0] + self.large[0]) / 2.0
```

### Time Complexity
- `addNum()`: O(log n), inserting into and balancing heaps
- `findMedian()`: O(1), extracting the median

### Space Complexity
- `O(n)`, where `n` is the number of elements added to the heaps

This approach efficiently maintains the balance between the two halves of the data, ensuring quick access to the median, which makes it well-suited for streaming data scenarios.

