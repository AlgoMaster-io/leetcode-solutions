# 56. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approach: Sort and Merge

### Solution
```python
# Time Complexity: O(n * log(n)), where n is the number of intervals (due to sorting)
# Space Complexity: O(n), for the result list
from typing import List

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals:
            return []

        # Step 1: Sort intervals by their start time
        intervals.sort(key=lambda x: x[0])

        # Step 2: Merge intervals
        merged = []
        current_interval = intervals[0]

        for i in range(1, len(intervals)):
            if intervals[i][0] <= current_interval[1]:
                # Overlapping intervals, merge them
                current_interval[1] = max(current_interval[1], intervals[i][1])
            else:
                # Non-overlapping interval, add the current interval to the result
                merged.append(current_interval)
                current_interval = intervals[i]

        # Add the last interval
        merged.append(current_interval)

        return merged
```

