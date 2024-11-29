# 57. [Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approach: Iteration and Merge

### Solution
```python
# Time Complexity: O(n), where n is the number of intervals
# Space Complexity: O(n), for the result list

def insert(intervals, newInterval):
    result = []
    i, n = 0, len(intervals)
    
    # Step 1: Add all intervals that come before the new interval
    while i < n and intervals[i][1] < newInterval[0]:
        result.append(intervals[i])
        i += 1
    
    # Step 2: Merge overlapping intervals with the new interval
    while i < n and intervals[i][0] <= newInterval[1]:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
        i += 1
    result.append(newInterval)  # Add the merged interval
    
    # Step 3: Add all intervals that come after the new interval
    while i < n:
        result.append(intervals[i])
        i += 1
    
    return result
```

