# [Leetcode 57: Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Improved Merge Approach](#approach-2-improved-merge-approach)

### Approach 1: Brute Force

**Intuition**:  
The simplest way to solve this problem is to:
1. Add the new interval to the list of intervals.
2. Sort the intervals by their start.
3. Traverse the sorted intervals and merge any overlapping intervals.

This method ensures that we first arrange intervals using the proper sequence and then combine any overlapping intervals.

**Algorithm**:
1. Add the `newInterval` to the `intervals` list.
2. Sort the list by the start time of each interval.
3. Initialize a result list with the first interval.
4. Iterate through the list of intervals:
   - If the current interval overlaps with the last interval in the result list, merge them.
   - Otherwise, add the current interval to the result list.

```go
func insert(intervals [][]int, newInterval []int) [][]int {
    // Step 1: Add the new interval
    intervals = append(intervals, newInterval)

    // Step 2: Sort intervals by the start times
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })

    // Step 3: Use a result list to manage merged intervals
    result := [][]int{intervals[0]}

    // Step 4: Iterate over the intervals after the first
    for i := 1; i < len(intervals); i++ {
        lastInterval := result[len(result)-1]
        currInterval := intervals[i]

        // If the current interval overlaps with the last one in the result, merge them
        if currInterval[0] <= lastInterval[1] {
            lastInterval[1] = max(lastInterval[1], currInterval[1])
        } else {
            // Else, add the current interval as it is
            result = append(result, currInterval)
        }
    }

    return result
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

**Time Complexity**: O(n log n) where n is the number of intervals initially present. This complexity is due to the sorting step.
**Space Complexity**: O(n) for storing the merged intervals.

### Approach 2: Improved Merge Approach

**Intuition**:  
Instead of sorting and then iterating, we can optimize by inserting the new interval in the correct position to save on sorting time. Then, we simply merge while processing the intervals once. 

**Algorithm**:
1. Initialize an empty list for the result.
2. Traverse the list of intervals:
   - If the current interval ends before `newInterval` starts, add it to the result.
   - If the current interval starts after `newInterval` ends, add the `newInterval` to the result, then all subsequent intervals.
   - If the intervals overlap, merge them.
3. Ensure the `newInterval` is added if isolated.

```go
func insert(intervals [][]int, newInterval []int) [][]int {
    var result [][]int
    i := 0
    n := len(intervals)

    // Step 1: Add all intervals ending before newInterval starts
    for i < n && intervals[i][1] < newInterval[0] {
        result = append(result, intervals[i])
        i++
    }

    // Step 2: Merge overlapping intervals with newInterval
    for i < n && intervals[i][0] <= newInterval[1] {
        newInterval[0] = min(intervals[i][0], newInterval[0])
        newInterval[1] = max(intervals[i][1], newInterval[1])
        i++
    }
    
    // Step 3: Add the merged newInterval
    result = append(result, newInterval)

    // Step 4: Add all intervals starting after newInterval ends
    for i < n {
        result = append(result, intervals[i])
        i++
    }

    return result
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

**Time Complexity**: O(n), where n is the number of intervals. We go through the intervals just once.
**Space Complexity**: O(n) for the output storage.

