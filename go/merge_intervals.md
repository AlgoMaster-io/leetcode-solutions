# 56. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approach: Sort and Merge

### Solution
go
```go
// Time Complexity: O(n * log(n)), where n is the number of intervals (due to sorting)
// Space Complexity: O(n), for the result slice
import (
    "sort"
)

func merge(intervals [][]int) [][]int {
    if len(intervals) == 0 {
        return [][]int{}
    }

    // Step 1: Sort intervals by their start time
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })

    // Step 2: Merge intervals
    merged := [][]int{}
    currentInterval := intervals[0]

    for i := 1; i < len(intervals); i++ {
        if intervals[i][0] <= currentInterval[1] {
            // Overlapping intervals, merge them
            if intervals[i][1] > currentInterval[1] {
                currentInterval[1] = intervals[i][1]
            }
        } else {
            // Non-overlapping interval, add the current interval to the result
            merged = append(merged, currentInterval)
            currentInterval = intervals[i]
        }
    }

    // Add the last interval
    merged = append(merged, currentInterval)

    return merged
}
```

