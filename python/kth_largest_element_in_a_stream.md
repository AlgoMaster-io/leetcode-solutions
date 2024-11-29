# 703. [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approach: Min-Heap

### Solution
python
```python
# Time Complexity: O(n * log(k)) for initialization and O(log(k)) for each add operation
# Space Complexity: O(k), where k is the size of the heap
import heapq

class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.minHeap = []  # Min-heap to store the k largest elements
        
        # Add all elements to the heap and maintain only k elements
        for num in nums:
            self.add(num)

    def add(self, val: int) -> int:
        heapq.heappush(self.minHeap, val)  # Add the new value to the heap

        # Remove the smallest element if the heap exceeds size k
        if len(self.minHeap) > self.k:
            heapq.heappop(self.minHeap)
        
        return self.minHeap[0]  # The kth largest element is the root of the heap

# Usage:
# obj = KthLargest(k, nums)
# param_1 = obj.add(val)
```

