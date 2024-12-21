# [Leetcode 56: Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approaches
1. [Merge Intervals using Sorting and Iteration](#approach-1-merge-intervals-using-sorting-and-iteration)

---

## Approach 1: Merge Intervals using Sorting and Iteration

### Intuition

The problem of merging overlapping intervals can be visualized as laying books end-to-end on a shelf. When some books' bindings overlap, you only need to see the start of the first book to the end of the last book to know where the books start and end as a group.

The core idea of this approach is to sort the intervals based on their start times. Once sorted, we can compare each subsequent interval to the last interval in our result list. If they overlap, we merge them into a single continuous interval. Otherwise, if they don't overlap, we add the current interval to our result list as it is.

### Steps:

1. **Sort Intervals:** First, sort the intervals based on the start time. This enables us to easily verify overlap since a potential overlap can only occur with the immediate next interval once sorted.

2. **Iterate & Merge:** Start iterating over the sorted intervals, keeping track of the current interval we're merging (or the last interval in the merged list).
   - If the current interval overlaps with the last merged interval, merge them by updating the end of the last merged interval to the maximum of both.
   - If there is no overlap, push the current interval into the result list as a new entry.

3. **Return Result:** After iterating through all intervals, the result list will contain all merged intervals.

### Code

```javascript
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function(intervals) {
    // Sort intervals based on the starting time
    intervals.sort((a, b) => a[0] - b[0]);

    // Initialize result array for merged intervals
    let merged = [];

    // Iterate over each interval
    for (let i = 0; i < intervals.length; i++) {
        // Get the current interval
        let currentInterval = intervals[i];

        // If merged list is empty or the current interval does not overlap with the last merged interval:
        if (merged.length === 0 || merged[merged.length - 1][1] < currentInterval[0]) {
            merged.push(currentInterval); // Add the current interval into merged list
        } else {
            // If it overlaps, merge the intervals
            // Update the end time of the last interval in 'merged' to be the max end time
            merged[merged.length - 1][1] = Math.max(merged[merged.length - 1][1], currentInterval[1]);
        }
    }

    return merged;
};
```

### Complexity Analysis

- **Time Complexity:** \(O(n \log n)\), where \(n\) is the number of intervals. Sorting the intervals takes \(O(n \log n)\), and then we perform a single pass over the intervals.
- **Space Complexity:** \(O(\log n)\) to \(O(n)\), depending on the sorting algorithm used. The additional space complexity incurred by the storage of the intervals is \(O(n)\) in the worst case.

This approach efficiently processes the intervals and produces merged intervals with minimal additional complexity.

