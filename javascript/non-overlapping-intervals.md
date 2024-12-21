# [Leetcode 435: Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

## Table of Contents
1. [Greedy Approach](#greedy-approach)

## Greedy Approach

### Intuition
The problem asks us to remove the minimum number of intervals to make the rest non-overlapping. A common strategy for problems involving intervals is sorting. Here, if we sort the intervals based on their end times, we can always choose the interval that ends earliest, which leaves the maximum possibility for other intervals to fit into the schedule. This greedy approach optimizes for these opportunities and ultimately minimizes the number of removals needed.

### Detailed Steps
1. **Sort the intervals based on their end times**: By ordering this way, we can ensure we're always checking the interval that ends the soonest.
2. **Initialize counters**: Use `count` for the number of non-overlapping intervals included in our solution, and `end` to keep track of the end time of the last interval added to the solution.
3. **Iterate over the intervals**:
   - If the start of the current interval is greater or equal to `end`, it means this interval does not overlap with the previous one included. Therefore, we update `end` to the current interval's end and increment `count`.
   - If the start is less than `end`, this interval overlaps with the previously included one and needs to be removed. In this situation, we do not update `end` and do not increment `count`.
4. The difference between the total number of intervals and `count` will give us the minimum number of intervals to remove for all remaining intervals to be non-overlapping.

Here's the implementation in JavaScript:

```javascript
var eraseOverlapIntervals = function(intervals) {
    if (intervals.length === 0) return 0;
    
    // Sort intervals based on the end time
    intervals.sort((a, b) => a[1] - b[1]);
    
    let count = 1;
    let end = intervals[0][1];
    
    for (let i = 1; i < intervals.length; i++) {
        // Check if the current interval does not overlap
        if (intervals[i][0] >= end) {
            count++;
            end = intervals[i][1];
        }
    }
    
    // The number of intervals to remove is total intervals - non-overlapping intervals count
    return intervals.length - count;
};
```

### Time Complexity
- Sorting the intervals takes \(O(n \log n)\).
- Iterating through the list of intervals takes \(O(n)\).
- So, the overall time complexity is \(O(n \log n)\).

### Space Complexity
- We sort the intervals in-place, hence only using \(O(1)\) additional space. The overall space complexity remains \(O(1)\).

This approach leverages a greedy algorithm to handle overlapping intervals effectively while ensuring the solution is efficient with respect to time and space.

