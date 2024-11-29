# [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approach 1: HashMap and Min-Heap

### Solution
```python
# Time Complexity: O(n log k)
# Space Complexity: O(n)
import heapq
from collections import defaultdict

class Solution:
    def topKFrequent(self, words, k):
        # Count the frequency of each word
        frequency_map = defaultdict(int)
        for word in words:
            frequency_map[word] += 1

        # Use a min-heap to keep the top k frequent words
        min_heap = []
        for word in frequency_map:
            heapq.heappush(min_heap, (frequency_map[word], word))
            if len(min_heap) > k:
                heapq.heappop(min_heap)

        # Build the result list from the heap
        result = []
        while min_heap:
            result.append(heapq.heappop(min_heap)[1])
        
        return result[::-1] # Reverse to get the most frequent words first
```

## Approach 2: HashMap and Sorting

### Solution
```python
# Time Complexity: O(n + k log k)
# Space Complexity: O(n)
from collections import defaultdict

class Solution:
    def topKFrequent(self, words, k):
        # Count the frequency of each word
        frequency_map = defaultdict(int)
        for word in words:
            frequency_map[word] += 1

        # Sort the words by frequency and lexicographical order
        candidates = sorted(frequency_map.keys(), key=lambda word: (-frequency_map[word], word))

        # Return the top k words
        return candidates[:k]
```

