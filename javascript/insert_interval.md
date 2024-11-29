# 57. [Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approach: Iteration and Merge

### Solution
```javascript
// Time Complexity: O(n), where n is the number of intervals
// Space Complexity: O(n), for the result array

function insert(intervals, newInterval) {
    const result = [];
    let i = 0, n = intervals.length;

    // Step 1: Add all intervals that come before the new interval
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.push(intervals[i]);
        i++;
    }

    // Step 2: Merge overlapping intervals with the new interval
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.push(newInterval); // Add the merged interval

    // Step 3: Add all intervals that come after the new interval
    while (i < n) {
        result.push(intervals[i]);
        i++;
    }

    return result; // Return the result array
}
```

