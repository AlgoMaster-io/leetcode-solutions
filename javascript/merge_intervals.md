# 56. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approach: Sort and Merge

### Solution
javascript
```javascript
// Time Complexity: O(n * log(n)), where n is the number of intervals (due to sorting)
// Space Complexity: O(n), for the result list

function merge(intervals) {
    if (intervals == null || intervals.length === 0) {
        return [];
    }

    // Step 1: Sort intervals by their start time
    intervals.sort((a, b) => a[0] - b[0]);

    // Step 2: Merge intervals
    const merged = [];
    let currentInterval = intervals[0];

    for (let i = 1; i < intervals.length; i++) {
        if (intervals[i][0] <= currentInterval[1]) {
            // Overlapping intervals, merge them
            currentInterval[1] = Math.max(currentInterval[1], intervals[i][1]);
        } else {
            // Non-overlapping interval, add the current interval to the result
            merged.push(currentInterval);
            currentInterval = intervals[i];
        }
    }

    // Add the last interval
    merged.push(currentInterval);

    return merged;
}
```

