# [Leetcode 435: Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

## Approaches
1. [Greedy Approach](#greedy-approach)

---

## Greedy Approach

The problem at hand is to determine the minimum number of intervals to remove so that the remaining intervals don't overlap. A greedy approach can be applied here, as sorting the intervals and making local optimal choices at each step will lead us to the optimal global solution.

### Intuition

1. **Sort by End Times**: The idea is to minimize the number of intervals to remove by trying to hold onto intervals that finish the earliest. Sorting based on the end times of intervals ensures that we can build upon the interval that leaves the least room for future overlaps.

2. **Greedy Choice**: Once sorted by end time, iterate over the intervals, and keep track of the end time of the last interval added to the non-overlapping set. If the start time of the current interval is less than the end time of the chosen interval, it implies an overlap and hence that interval should be discarded.

3. **Count Overlaps**: Count the number of such overlaps/deletions which will represent the minimal intervals we need to remove.

### Detailed Steps

- Sort the intervals based on their end times.
- Initialize a variable to keep track of the end of the "last added" non-overlapping interval.
- Iterate over the sorted intervals and check if the current interval's start time is greater than or equal to the last interval's end time.
  - If yes, update the last added interval end time to the current interval's end time.
  - If no, increment the removal count.

### Code

```go
func eraseOverlapIntervals(intervals [][]int) int {
    // Sort intervals by their end values
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][1] < intervals[j][1]
    })

    // Variable to keep track of the count of intervals to remove
    count := 0
    
    // Initialize a variable to hold the end of the last interval in the non-overlapping set
    end := intervals[0][1]

    // Iterate over intervals starting from the second one
    for i := 1; i < len(intervals); i++ {
        // If there is an overlap
        if intervals[i][0] < end {
            // Increment the count as we need to remove this overlapping interval
            count++
        } else {
            // Else, update the end to the current interval's end
            end = intervals[i][1]
        }
    }

    return count
}
```

### Complexity Analysis

- **Time Complexity**: `O(n log n)`, where `n` is the number of intervals. Sorting the intervals takes `O(n log n)`, and iterating through them takes `O(n)`.
  
- **Space Complexity**: `O(1)` or `O(n)` depending on the sorting implementation. The additional O(1) space is for variable storage, but sorting may require O(n) to create a copy of the array or stack space in more advanced sorting algorithms.

In summary, the greedy algorithm efficiently solves the problem by focusing on the non-overlapping intervals that allow maximum subsequent engagements, ensuring the minimal number of deletions for a solution.

