# [Leetcode 56: Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approaches
- [Approach 1: Basic Merge with Sorting](#approach-1-basic-merge-with-sorting)
- [Approach 2: Optimal Merge with Sorting and Single Pass](#approach-2-optimal-merge-with-sorting-and-single-pass)

## Approach 1: Basic Merge with Sorting

### Intuition
The basic approach involves sorting the intervals based on their starting times. Once sorted, we can iterate through the list and merge intervals that overlap. Overlapping intervals are intervals where the end of one interval is greater than or equal to the start of the next.

### Steps
1. **Sort the Intervals**: Start by sorting the intervals by the start time. This ensures that all overlapping intervals are adjacent.
2. **Iterate and Merge**: Traverse through the sorted intervals and keep merging overlapping intervals. If an interval does not overlap, simply add it to the result.

### Code
```typescript
function merge(intervals: number[][]): number[][] {
    // Step 1: Sort the intervals based on the starting time
    intervals.sort((a, b) => a[0] - b[0]);

    const merged: number[][] = [];

    for (const interval of intervals) {
        // Step 2: If merged is empty or the current interval does not overlap with the last one in 'merged'
        if (merged.length === 0 || merged[merged.length - 1][1] < interval[0]) {
            // Simply add the interval to 'merged'
            merged.push(interval);
        } else {
            // There is an overlap; merge the current and last intervals in 'merged'
            merged[merged.length - 1][1] = Math.max(merged[merged.length - 1][1], interval[1]);
        }
    }

    return merged;
}
```

### Complexity Analysis
- **Time Complexity**: O(n log n), where n is the number of intervals. The time is primarily dominated by the sorting step.
- **Space Complexity**: O(n), as we might need to store all intervals in worst case in the merged array.

## Approach 2: Optimal Merge with Sorting and Single Pass

### Intuition
The previous approach is quite efficient, and not much optimization can be done over it. However, we can articulate a more concise and efficient way to manage the merging logic for a slightly cleaner implementation. This approach does not add tangible time or space complexity reductions beyond constant factors.

### Steps
1. **Sort the Intervals**: As before, the first step is sorting the intervals by starting time.
2. **Iterate**: Sweep through the intervals and maintain a single `currentInterval` which represents the active interval being merged.
3. **Merging Logic**: When you find an interval that does not overlap, push the `currentInterval` into `merged` and update `currentInterval`.

### Code
```typescript
function merge(intervals: number[][]): number[][] {
    // Sort intervals by their starting position
    intervals.sort((a, b) => a[0] - b[0]);

    const merged: number[][] = [];
    let currentInterval = intervals[0];

    for (let i = 1; i < intervals.length; i++) {
        const interval = intervals[i];
        
        // Check for overlap with currentInterval
        if (interval[0] <= currentInterval[1]) {
            // Merge the intervals
            currentInterval[1] = Math.max(currentInterval[1], interval[1]);
        } else {
            // No overlap, push currentInterval and reset it
            merged.push(currentInterval);
            currentInterval = interval;
        }
    }

    // Don't forget to push the last interval
    merged.push(currentInterval);

    return merged;
}
```

### Complexity Analysis
- **Time Complexity**: O(n log n), due to sorting.
- **Space Complexity**: O(n), due to the array to hold merged intervals.

This encapsulates the different ways to solve the Merge Intervals problem effectively. Both approaches are efficient, predominantly governed by the requirement to sort the intervals initially.

