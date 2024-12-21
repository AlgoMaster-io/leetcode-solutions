# [Leetcode 57: Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approaches

1. [Basic Approach: Iterative Insertion](#basic-approach-iterative-insertion)
2. [Optimal Approach: Merging Overlapping Intervals](#optimal-approach-merging-overlapping-intervals)

---

### Basic Approach: Iterative Insertion

**Intuition:**

The basic idea is to iterate through the list of intervals and determine where to insert the new interval. We append the intervals to the result list as follows:
- If the current interval ends before the new interval starts, then the current interval does not overlap with the new interval.
- If the current interval starts after the new interval ends, then there's no more overlap possible, and we can add the rest of the intervals as is.

**Steps:**

1. Initialize a result array to hold the final intervals.
2. Iterate through each interval and check if it overlaps with the new interval.
3. If an interval does not overlap and is before the new interval, add it to the result.
4. If an interval does overlap, merge it with the new interval.
5. Once you find an interval that starts after the new interval ends, add the new interval to the result and continue adding the rest of the intervals.

**Time Complexity:** O(N)  
**Space Complexity:** O(N) where N is the number of intervals.

```typescript
function insert(intervals: number[][], newInterval: number[]): number[][] {
    const result: number[][] = [];
    let i = 0;
    const n = intervals.length;

    // Add all intervals that end before the new interval starts
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.push(intervals[i]);
        i++;
    }

    // Merge all overlapping intervals to one considering the newInterval
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.push(newInterval);

    // Add all the rest intervals
    while (i < n) {
        result.push(intervals[i]);
        i++;
    }

    return result;
}
```

---

### Optimal Approach: Merging Overlapping Intervals

**Intuition:**

This approach is more or less similar to the basic one but can be slightly optimized in terms of readability. The primary focus is on merging intervals that overlap with the new interval and inserting the new interval at the correct position.

**Steps:**

1. Initialize a result array and a variable `i` for tracking the index of the current interval.
2. Loop through intervals:
   - Add intervals that do not overlap and are before the new interval.
   - Adjust the start and end of the new interval by merging overlapping intervals.
   - Once all potential overlaps are processed, add the merged new interval.
3. Add any remaining intervals after the new interval is added to the result.

This does not change the time complexity but makes our operations more explicit.

**Time Complexity:** O(N)  
**Space Complexity:** O(N)

```typescript
function insert(intervals: number[][], newInterval: number[]): number[][] {
    const result: number[][] = [];
    let i = 0;
    const n = intervals.length;

    // Insert all intervals that come before the new interval
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.push(intervals[i]);
        i++;
    }

    // Merge overlapping intervals into the new interval
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.push(newInterval);

    // Insert remaining intervals
    while (i < n) {
        result.push(intervals[i]);
        i++;
    }

    return result;
}
```

This solution efficiently combines interval insertion and merging, resulting in a cleaned and concise result.

