# [Leetcode Problem 57: Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Efficient Approach Using Merge Overlapping Intervals Principles](#efficient-approach-using-merge-overlapping-intervals-principles)

### Brute Force Approach

#### Intuition
The idea is to find the right position where the new interval can be inserted to keep the list sorted based on the start times. Once found, iterate through the merged list of intervals to merge overlapping intervals. This is a straightforward approach that involves two passes through the intervals list.

#### Code
```javascript
function insert(intervals, newInterval) {
    // Add the new interval to the list of existing intervals
    intervals.push(newInterval);
    // Sort intervals based on the start time
    intervals.sort((a, b) => a[0] - b[0]);
    
    let merged = []; // This will store the final merged intervals
    for (let interval of intervals) {
        // If merged is empty or there is no overlap with the last interval in merged
        if (merged.length === 0 || merged[merged.length - 1][1] < interval[0]) {
            merged.push(interval); // Add the current interval
        } else {
            // There's overlap, so we need to merge intervals
            merged[merged.length - 1][1] = Math.max(merged[merged.length - 1][1], interval[1]);
        }
    }
    
    return merged;
}
```

#### Complexity
- **Time Complexity**: O(n log n) due to sorting of intervals.
- **Space Complexity**: O(n) for storing the merged intervals.

### Efficient Approach Using Merge Overlapping Intervals Principles

#### Intuition
A more efficient approach involves finding insertion points by making one pass through the list of intervals. The new interval can be directly merged with overlapping intervals as we iterate through the list. This requires fewer iterations and avoids the need to sort, making it more optimal.

#### Code
```javascript
function insert(intervals, newInterval) {
    let merged = [];
    let i = 0;
    let n = intervals.length;
    
    // Add all intervals coming before the newInterval
    while (i < n && intervals[i][1] < newInterval[0]) {
        merged.push(intervals[i]);
        i++;
    }

    // Merge all overlapping intervals to one considering newInterval
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    // Add the merged interval
    merged.push(newInterval);

    // Add all the remaining intervals
    while (i < n) {
        merged.push(intervals[i]);
        i++;
    }
    
    return merged;
}
```

#### Complexity
- **Time Complexity**: O(n) as we are going through the intervals list a constant number of times.
- **Space Complexity**: O(n) for storing the merged intervals.

