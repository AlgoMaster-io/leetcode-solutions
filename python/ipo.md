# 480. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approach: Two Heaps (Max-Heap and Min-Heap)

### Solution

python
```python
# Time Complexity: O(n * log(k)), where n is the size of nums and k is the size of the window
# Space Complexity: O(k), for storing elements in the heaps
import heapq

class Solution:
    def medianSlidingWindow(self, nums, k):
        n = len(nums)
        result = [0] * (n - k + 1)
        
        # Max-heap for the smaller half
        max_heap = []
        # Min-heap for the larger half
        min_heap = []
        
        for i in range(n):
            # Add the new element into the appropriate heap
            if not max_heap or nums[i] <= -max_heap[0]:
                heapq.heappush(max_heap, -nums[i])
            else:
                heapq.heappush(min_heap, nums[i])
            
            # Rebalance the heaps if necessary
            if len(max_heap) > len(min_heap) + 1:
                heapq.heappush(min_heap, -heapq.heappop(max_heap))
            elif len(min_heap) > len(max_heap):
                heapq.heappush(max_heap, -heapq.heappop(min_heap))
            
            # Remove the element that is sliding out of the window
            if i >= k:
                element_to_remove = nums[i - k]
                if element_to_remove <= -max_heap[0]:
                    max_heap.remove(-element_to_remove)
                    heapq.heapify(max_heap)
                else:
                    min_heap.remove(element_to_remove)
                    heapq.heapify(min_heap)
                
                # Rebalance the heaps after removal
                if len(max_heap) > len(min_heap) + 1:
                    heapq.heappush(min_heap, -heapq.heappop(max_heap))
                elif len(min_heap) > len(max_heap):
                    heapq.heappush(max_heap, -heapq.heappop(min_heap))
            
            # Add the median to the result when the window is fully formed
            if i >= k - 1:
                if len(max_heap) == len(min_heap):
                    result[i - k + 1] = (-max_heap[0] + min_heap[0]) / 2
                else:
                    result[i - k + 1] = -max_heap[0]
        
        return result
```

