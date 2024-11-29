# 295. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approach: Two Heaps

### Solution
python
```python
# Time Complexity: O(log(n)) for adding a number, O(1) for finding the median
# Space Complexity: O(n), where n is the number of elements in the data stream
import heapq

class MedianFinder:
    def __init__(self):
        self.maxHeap = []  # Max-heap (invert values to simulate max-heap)
        self.minHeap = []  # Min-heap

    def addNum(self, num):
        # Add to maxHeap (invert the number to simulate max-heap)
        heapq.heappush(self.maxHeap, -num)
        heapq.heappush(self.minHeap, -heapq.heappop(self.maxHeap))

        # Ensure maxHeap always has the same number or one more element than minHeap
        if len(self.maxHeap) < len(self.minHeap):
            heapq.heappush(self.maxHeap, -heapq.heappop(self.minHeap))

    def findMedian(self) -> float:
        if len(self.maxHeap) > len(self.minHeap):
            return -self.maxHeap[0]  # Odd number of elements
        return (-self.maxHeap[0] + self.minHeap[0]) / 2.0  # Even number of elements

# Example of usage:
# medianFinder = MedianFinder()
# medianFinder.addNum(num)
# print(medianFinder.findMedian())
```

