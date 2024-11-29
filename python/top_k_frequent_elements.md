# 347. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approach 1: Min-Heap

### Solution
```python
# Time Complexity: O(n * log(k)), where n is the number of elements in nums
# Space Complexity: O(n), for the frequency map and heap
import heapq
from collections import defaultdict

class Solution:
    def topKFrequent(self, nums, k):
        # Step 1: Build a frequency map
        frequency_map = defaultdict(int)
        for num in nums:
            frequency_map[num] += 1

        # Step 2: Use a min-heap to keep track of the top k elements
        min_heap = []
        
        for num, freq in frequency_map.items():
            heapq.heappush(min_heap, (freq, num))
            if len(min_heap) > k:
                heapq.heappop(min_heap)

        # Step 3: Extract elements from the heap
        result = []
        while min_heap:
            result.append(heapq.heappop(min_heap)[1])

        return result
```

## Approach 2: Bucket Sort

### Solution
```python
# Time Complexity: O(n), where n is the number of elements in nums
# Space Complexity: O(n), for the frequency map and buckets
from collections import defaultdict

class Solution:
    def topKFrequent(self, nums, k):
        # Step 1: Build a frequency map
        frequency_map = defaultdict(int)
        for num in nums:
            frequency_map[num] += 1

        # Step 2: Create buckets where index represents frequency
        buckets = [[] for _ in range(len(nums) + 1)]
        for num, freq in frequency_map.items():
            buckets[freq].append(num)

        # Step 3: Collect the top k frequent elements
        result = []
        for i in range(len(buckets) - 1, 0, -1):
            if buckets[i]:
                for num in buckets[i]:
                    result.append(num)
                    if len(result) == k:
                        return result
```

