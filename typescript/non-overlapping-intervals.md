# [Leetcode 435: Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

In this problem, we are given a collection of intervals and our task is to find the minimum number of intervals we need to remove to make the rest of the intervals non-overlapping.

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Greedy Approach](#greedy-approach)

### Brute Force Approach
The simplest way to solve this problem is by trying to remove each possible interval and check if the remaining intervals are non-overlapping. Although this approach is not efficient, it helps to build intuition.

#### Intuition:
1. Generate all possible subsets of the intervals by removing one interval at a time.
2. For each subset, check if the intervals are non-overlapping.
3. Keep track of the minimum number of elements removed to achieve a non-overlapping condition.

```typescript
function canAttendMeetings(intervals: number[][]): boolean {
    for (let i = 0; i < intervals.length; i++) {
        for (let j = i + 1; j < intervals.length; j++) {
            if (intervals[i][1] > intervals[j][0] && intervals[i][0] < intervals[j][1]) {
                return false;
            }
        }
    }
    return true;
}

function eraseOverlapIntervals(intervals: number[][]): number {
    let minRemovals = Infinity;

    const removeIntervals = (removedCount: number = 0, idx: number = 0) => {
        if (idx >= intervals.length) {
            if (canAttendMeetings(intervals)) {
                minRemovals = Math.min(minRemovals, removedCount);
            }
            return;
        }

        const currentIntervals = intervals.slice();
        // Remove current interval
        intervals.splice(idx, 1);
        removeIntervals(removedCount + 1, idx);
        intervals = currentIntervals.slice();
        // Skip current interval
        removeIntervals(removedCount, idx + 1);
    };

    removeIntervals();
    return minRemovals;
}
```

**Time Complexity:** \(O(n^2 \times 2^n)\)  
**Space Complexity:** \(O(n)\)

### Greedy Approach
A more optimal way to solve this problem is by using a greedy approach. The key insight is to always keep the interval that ends the earliest, as this will leave the most room for subsequent intervals.

#### Intuition:
1. Sort the intervals by their end times.
2. Iterate through the sorted intervals and keep track of the end of the last non-overlapping interval.
3. If the start of the current interval is less than the end of the last non-overlapping interval, it overlaps, and we increment the removal count.
4. Otherwise, we update the end to the end of the current interval.

```typescript
function eraseOverlapIntervalsGreedy(intervals: number[][]): number {
    if (intervals.length === 0) return 0;

    // Sort intervals based on end time
    intervals.sort((a, b) => a[1] - b[1]);

    let nonOverlappingCount = 0;
    let lastEnd = -Infinity;

    for (const [start, end] of intervals) {
        // If it does not overlap, update the lastEnd
        if (start >= lastEnd) {
            lastEnd = end;
        } else {
            // Otherwise, it is overlapping; we need to count it as a removal
            nonOverlappingCount++;
        }
    }

    return nonOverlappingCount;
}
```

**Time Complexity:** \(O(n \log n)\) due to sorting.  
**Space Complexity:** \(O(1)\), as we only use a fixed amount of additional space.

This greedy approach provides the optimal solution with minimal time and space complexity.

