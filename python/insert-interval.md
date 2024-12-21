# Problem: [57. Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized One Pass Merge](#optimized-one-pass-merge)

---

### Brute Force Approach

#### Intuition:
The brute force approach involves iterating through the list of intervals and placing the new interval in the correct position such that the intervals remain sorted by their starting times. Afterwards, we simply iterate through the updated list once again to merge any overlapping intervals.

#### Steps:
1. Insert the new interval into the intervals list at the appropriate position to maintain order.
2. Iterate through the list of intervals and insert any interval that does not overlap into a result list.
3. If there is any overlap, merge the intervals by updating the start and end accordingly.
4. Update the result list and repeat until all intervals are processed.

#### Time Complexity:
- **O(n log n)** due to sorting the intervals after insertion.
  
#### Space Complexity:
- **O(n)** since we may end up adding a new interval to the list, leading to an increase in memory usage proportional to n.

```python
def insert(intervals, newInterval):
    # Append the newInterval to preserve the input intervals list.
    intervals.append(newInterval)
    
    # Sort the intervals by their starting values.
    intervals.sort(key=lambda x: x[0])
    
    merged = []
    
    for interval in intervals:
        # If merged is empty or there is no overlap
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            # There is overlap, so merge the current interval with the last interval in merged
            merged[-1][1] = max(merged[-1][1], interval[1])
    
    return merged
```

---

### Optimized One Pass Merge

#### Intuition:
This approach bypasses the need to sort intervals after adding the new interval. Instead, it processes and merges intervals in a single pass while keeping track of the merging conditions. The algorithm integrates the new interval into the list directly while managing overlaps in real-time through conditional checks.

#### Steps:
1. Initialize a list to hold the merged intervals.
2. Traverse through the given intervals and manage the merging process for three conditions:
   - If the current interval ends before the new interval starts, add the current interval to the merged list.
   - If the new interval ends before the current interval starts, append the new interval to the merged list, then treat the current interval as a new starting point.
   - If intervals overlap, merge by updating the new interval start and end to be within the overlapping bounds.
3. Post iteration, if there is an unprocessed new interval, append it to the merged result.

#### Time Complexity:
- **O(n)** since we process each interval exactly once.

#### Space Complexity:
- **O(n)** because we construct a new list of intervals.

```python
def insert(intervals, newInterval):
    merged = []
    for interval in intervals:
        # Non-overlapping and ends before the new interval starts
        if interval[1] < newInterval[0]:
            merged.append(interval)
        # New interval ends before the current interval starts
        elif newInterval[1] < interval[0]:
            merged.append(newInterval)
            newInterval = interval
        # Overlapping intervals
        else:
            newInterval[0] = min(newInterval[0], interval[0])
            newInterval[1] = max(newInterval[1], interval[1])

    # Add last interval
    merged.append(newInterval)
    
    return merged
```

By understanding and utilizing these approaches, you'll develop both a comprehensive understanding of the problem and familiarity with the trade-offs between intuitive brute force solutions and more optimized algorithms!

