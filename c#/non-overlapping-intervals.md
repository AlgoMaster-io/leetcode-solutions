[Leetcode Problem 435: Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

### Approaches
- [Brute Force Approach](#brute-force-approach)
- [Greedy Approach](#greedy-approach)

---

### Brute Force Approach

The brute force approach involves checking every possible pair of intervals to see if they overlap. For each pair of overlapping intervals, we keep track of one that we consider removing to reduce further overlaps. However, this approach is not efficient and is used for simpler problems to verify our understanding.

#### Intuition:
1. Sort the intervals based on their starting times.
2. Compare each pair of intervals, and whenever an overlap is found, mark one to be removed.
3. Iteration continues until all intervals are checked.

#### Code:
```csharp
public int EraseOverlapIntervals(int[][] intervals) {
    // Step 1: Sort intervals based on starting times
    Array.Sort(intervals, (a, b) => a[0].CompareTo(b[0]));
    
    int removeCount = 0;
    int lastIntervalEnd = intervals[0][1];
    
    // Step 2: Iterate through intervals and check for overlaps
    for (int i = 1; i < intervals.Length; i++) {
        if (intervals[i][0] < lastIntervalEnd) {
            // Step 3: Overlap found, let's decide which one to remove
            removeCount++;
            // Choose the interval with the earliest end time
            lastIntervalEnd = Math.Min(lastIntervalEnd, intervals[i][1]);
        } else {
            // Update lastIntervalEnd if no overlap
            lastIntervalEnd = intervals[i][1];
        }
    }
    
    return removeCount;
}
```

#### Complexity:
- **Time Complexity**: O(n^2) or worse due to every possible overlap check.
- **Space Complexity**: O(1) since we only track counts and indices.

---

### Greedy Approach

The greedy approach leverages sorting to efficiently determine the minimum number of removals needed to eliminate overlaps. By always selecting the interval with the earliest end time when overlaps occur, we can reduce the number of removals.

#### Intuition:
1. Sort intervals by end time, rather than start time.
2. Keep track of the last added interval to a non-overlapping list.
3. For each interval, add it to the list if it doesn't overlap with the last added one.
4. If it does overlap, count it as removed and continue checking.

#### Code:
```csharp
public int EraseOverlapIntervals(int[][] intervals) {
    // Step 1: Sort intervals based on end times
    Array.Sort(intervals, (a, b) => a[1].CompareTo(b[1]));
    
    int lastIntervalEnd = intervals[0][1];
    int removeCount = 0;
    
    // Step 2: Iterate through intervals checking for overlaps
    for (int i = 1; i < intervals.Length; i++) {
        if (intervals[i][0] < lastIntervalEnd) {
            // Overlap detected, mark one for removal
            removeCount++;
        } else {
            // No overlap, update the end time to the current interval's end
            lastIntervalEnd = intervals[i][1];
        }
    }
    
    return removeCount;
}
```

#### Complexity:
- **Time Complexity**: O(n log n) due to sorting of intervals.
- **Space Complexity**: O(1) as we predominantly use constant space for counting and comparing. 

---

