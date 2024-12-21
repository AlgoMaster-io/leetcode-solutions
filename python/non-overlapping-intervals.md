# [Leetcode 435: Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

## Approaches
1. [Greedy Approach](#greedy-approach)
2. [Detailed Greedy Approach with Sorting](#detailed-greedy-approach-with-sorting)

## Greedy Approach

### Intuition
The problem asks us to remove the minimum number of intervals to make the rest of the intervals non-overlapping. The greedy strategy involves sorting the intervals by their end times and iteratively selecting intervals with non-overlapping properties.

The idea is to always select the next interval in such a way that it leaves the most room for the remaining intervals, which is achieved by choosing the interval with the earliest end time. By doing so, we minimize the chances of future intervals overlapping.

### Steps
1. Sort the intervals based on their end time.
2. Initialize the `end` variable to negative infinity to compare with the end of the first interval.
3. Traverse through the sorted intervals:
   - If the start of the current interval is greater than or equal to the `end`, it means it doesn't overlap with the previous interval that was counted. Hence, update the `end` and count it as a part of our non-overlapping segments.
   - If it overlaps, increase the count of removed intervals.

```python
def eraseOverlapIntervals(intervals):
    # Sort intervals based on end times
    intervals.sort(key=lambda x: x[1])
    
    # Initially no end is set, so we use negative infinity
    end = float('-inf')
    count_removed = 0
    
    # Iterate through each interval
    for interval in intervals:
        # Check if the current interval starts after or when the previous ends
        if interval[0] >= end:
            # Update the end to current interval's end time
            end = interval[1]
        else:
            # Interval overlapping, needs removal
            count_removed += 1
    
    # Return the count of removed intervals
    return count_removed
```

### Complexity
- **Time Complexity:** O(n log n), due to the sorting step.
- **Space Complexity:** O(1), or O(n) if counting the input space.

## Detailed Greedy Approach with Sorting

### Intuition
While the previous solution gives a general idea, this approach elaborates on why sorting by the end time makes it optimal. By sorting by the end time, each decision to keep or remove an interval is incremental and factors in the most immediate constraint (current end time). This ensures each step optimally resolves the current smallest overlapping problem, essentially leaving more room for potential future intervals.

### Steps
1. Sort intervals by end time to ensure that at each step of iteration, the choice to either keep or remove is made with respect to the soonest possible freedom for the next interval.
2. Initialize `end` as `-inf` and a counter for removed intervals.
3. Loop through intervals, updating or counting removals with respect to their overlap with the current `end`.

```python
def eraseOverlapIntervals(intervals):
    if not intervals:
        # No intervals, no need to remove anything
        return 0
    
    # Sort intervals based on their end times for optimal fitting
    intervals.sort(key=lambda x: x[1])
    
    end = intervals[0][1]  # Initialize the end using the first interval
    count_removed = 0      # Initialize the counter for removed intervals
    
    for i in range(1, len(intervals)):
        if intervals[i][0] < end:
            # Overlapping interval found, increment removal counter
            count_removed += 1
        else:
            # Non-overlapping interval, update end
            end = intervals[i][1]
    
    return count_removed
```

### Complexity
- **Time Complexity:** O(n log n), owing to the sorting operation.
- **Space Complexity:** O(1), aside from the space used by the input list.

The greedy approach ensures that each segment of overlapping potential is addressed with priority on the earliest possible freedom for future intervals, adhering to the optimal substructure requisite for a greedy solution.

