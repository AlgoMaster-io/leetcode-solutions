# 354. [Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approach 1: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
from typing import List

class Solution:
    def maxEnvelopes(self, envelopes: List[List[int]]) -> int:
        if not envelopes:
            return 0

        # Sort envelopes by width; if width is the same, sort by height descending
        envelopes.sort(key=lambda x: (x[0], -x[1]))

        n = len(envelopes)
        dp = [1] * n
        max_envelopes = 0

        # Dynamic programming to find the maximum number of envelopes
        for i in range(n):
            for j in range(i):
                # Check if j can fit into i
                if envelopes[j][1] < envelopes[i][1] and envelopes[j][0] < envelopes[i][0]:
                    dp[i] = max(dp[i], dp[j] + 1)
            max_envelopes = max(max_envelopes, dp[i])

        return max_envelopes
```

## Approach 2: Patience Sorting and Longest Increasing Subsequence (LIS)

### Solution
python
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
from typing import List
import bisect

class Solution:
    def maxEnvelopes(self, envelopes: List[List[int]]) -> int:
        if not envelopes:
            return 0

        # Sort envelopes by width; if width is the same, sort by height descending
        envelopes.sort(key=lambda x: (x[0], -x[1]))

        # Array to hold our increasing sequence of heights
        lis = []

        for _, height in envelopes:
            # Perform binary search to find the correct position to replace or add
            pos = bisect.bisect_left(lis, height)

            # Add or replace the value at the identified position
            if pos < len(lis):
                lis[pos] = height
            else:
                lis.append(height)
        
        return len(lis)
```

