# [767. Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approach 1: Max Heap

### Solution
```python
# Time Complexity: O(n log k), where k is the number of unique characters
# Space Complexity: O(n + k)
import heapq
from collections import Counter

class Solution:
    def reorganizeString(self, s: str) -> str:
        # Frequency map for characters
        char_counts = Counter(s)

        # Priority queue to keep the most frequent character at the top
        max_heap = [(-count, char) for char, count in char_counts.items()]
        heapq.heapify(max_heap)

        result = []
        prev_count, prev_char = 0, ''

        while max_heap:
            count, char = heapq.heappop(max_heap)

            # Append current character
            result.append(char)

            # Re-add the previous character if it is still valid
            if prev_count < 0:
                heapq.heappush(max_heap, (prev_count, prev_char))

            # Update the previous character
            prev_count, prev_char = count + 1, char

        # If the result length is not the same as the input, return ""
        return ''.join(result) if len(result) == len(s) else ''
```

## Approach 2: Greedy with Frequency Array

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1) (assuming a fixed alphabet size)
class Solution:
    def reorganizeString(self, s: str) -> str:
        char_counts = Counter(s)
        max_count = max(char_counts.values())
        max_char = max(char_counts, key=char_counts.get)

        # If the most frequent character cannot fit, return ""
        if max_count > (len(s) + 1) // 2:
            return ""

        result = [''] * len(s)
        index = 0

        # Place the most frequent character at even indices
        while char_counts[max_char] > 0:
            result[index] = max_char
            index += 2
            char_counts[max_char] -= 1

        # Place the remaining characters
        for char, count in char_counts.items():
            while count > 0:
                if index >= len(result):
                    index = 1  # Start filling odd indices
                result[index] = char
                index += 2
                count -= 1

        return ''.join(result)
```

