# [Leetcode 56: Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approaches

- [Approach 1: Basic Sorting Approach](#approach-1-basic-sorting-approach)
- [Approach 2: Optimized Merge Using Sorting and Overlapping Check](#approach-2-optimized-merge-using-sorting-and-overlapping-check)

### Approach 1: Basic Sorting Approach

**Intuition:**
The basic idea here is to first sort the intervals. Once sorted, we can assume that each interval starts from a point which is not less than any other previous interval's start point. This allows us to easily manage merging by just checking the current interval's start with the last added interval's end.

1. **Sort the intervals** based on the start times. This will allow us to linearly process them and easily decide on merging.
2. **Initialize a result list** with the first interval.
3. **Iterate over each interval** starting from the second one:
    - If the current interval's start is less than or equal to the last merged interval's end, it means there's an overlap, so merge them by taking the maximum of the end points.
    - Else, there's no overlap, so add the current interval directly to the result list.
4. **Return the result list** since it contains all merged intervals.

```python
def merge(intervals):
    # Step 1: Sort the intervals based on the starting time
    intervals.sort(key=lambda x: x[0])
    
    # Step 2: Initialize the list to store merged intervals
    merged = [intervals[0]]
    
    for i in range(1, len(intervals)):
        # previous interval in the merged list
        prev_start, prev_end = merged[-1]
        # current interval to compare with
        current_start, current_end = intervals[i]
        
        # Check if the current interval overlaps with the previous one
        if current_start <= prev_end:
            # Merge the intervals by updating the last element of merged
            merged[-1][1] = max(prev_end, current_end)
        else:
            # No overlap, add the current interval to the merged list
            merged.append(intervals[i])
    
    return merged

# Time Complexity: O(N log N) due to sorting, where N is the number of intervals.
# Space Complexity: O(N) for the output array, in the worst-case scenario where no intervals overlap.
```

### Approach 2: Optimized Merge Using Sorting and Overlapping Check

**Intuition:**
This approach takes the essence of the first approach but is presented in a slightly more refined form, emphasizing the transition between merging intervals with direct comparison.

1. **Sort the intervals** on the basis of their starting times.
2. Maintain a running merged interval which will be compared with the upcoming intervals for potential merging.
3. Iterate over the intervals:
   - Check overlap using the end of the running merged interval.
   - Update the end of the running merged interval or add the current interval to the result list if no overlap.
4. Append the new intervals to keep the merged result updated.
5. Return the merged intervals which holds the non-overlapping intervals.

```python
def merge(intervals):
    # Step 1: Sort intervals based on first element of each interval (start time)
    intervals.sort(key=lambda x: x[0])

    merged = []
    
    for interval in intervals:
        # If merged is empty or there's no overlap, add the interval
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            # There is an overlap, so merge with the last interval in merged
            merged[-1][1] = max(merged[-1][1], interval[1])
    
    return merged

# Time Complexity: O(N log N) due to sorting. The iteration over the intervals is O(N).
# Space Complexity: O(N) for the storage of results in merged.
```

Both approaches use sorting which forms the time complexity's bottleneck. The essential improvement from the basic approach is directly iterating with checks and logical flow that make the merging more presentable and structured.

