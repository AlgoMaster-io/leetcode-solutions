# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approach 1: Using HashMap and Sorting

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
from collections import Counter
import heapq

class Solution:
    def frequencySort(self, s: str) -> str:
        # Count the frequency of each character
        frequency_map = Counter(s)

        # Sort characters by frequency in descending order
        max_heap = [(-freq, char) for char, freq in frequency_map.items()]
        heapq.heapify(max_heap)

        # Build the result string
        result = []
        while max_heap:
            freq, char = heapq.heappop(max_heap)
            result.append(char * -freq)

        return ''.join(result)
```

## Approach 2: Bucket Sort (Optimized for Large Inputs)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import defaultdict

class Solution:
    def frequencySort(self, s: str) -> str:
        # Count the frequency of each character
        frequency = [0] * 128  # Assuming ASCII characters
        for char in s:
            frequency[ord(char)] += 1

        # Create buckets where index represents frequency
        buckets = defaultdict(list)
        for i in range(len(frequency)):
            if frequency[i] > 0:
                buckets[frequency[i]].append(chr(i))

        # Build the result string from the buckets
        result = []
        for freq in range(len(s), 0, -1):
            if freq in buckets:
                for char in buckets[freq]:
                    result.append(char * freq)

        return ''.join(result)
```

