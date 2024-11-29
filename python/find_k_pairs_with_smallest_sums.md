# 373. [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approach: Min-Heap

### Solution
python
```python
# Time Complexity: O(k * log(k)), where k is the number of pairs to find
# Space Complexity: O(k), for the heap
import heapq

class Solution:
    def kSmallestPairs(self, nums1, nums2, k):
        # Min-heap to store pairs along with their sums
        minHeap = []
        
        # Initialize the heap with pairs (nums1[i], nums2[0]) for all i
        for i in range(min(len(nums1), k)):
            heapq.heappush(minHeap, (nums1[i] + nums2[0], nums1[i], nums2[0], 0))
        
        result = []
        
        # Extract the smallest pairs until we have k pairs or the heap is empty
        while minHeap and k > 0:
            _, num1, num2, index2 = heapq.heappop(minHeap)
            result.append([num1, num2])
            k -= 1
            
            # If there are more pairs with the current nums1[i], add them to the heap
            if index2 + 1 < len(nums2):
                heapq.heappush(minHeap, (num1 + nums2[index2 + 1], num1, nums2[index2 + 1], index2 + 1))
        
        return result
```

