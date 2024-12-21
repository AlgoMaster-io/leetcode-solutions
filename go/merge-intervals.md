# [Leetcode 56: Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approaches:
1. [Naive Approach - Pairwise Merging](#naive-approach---pairwise-merging)
2. [Optimized Approach - Sorting and Merging](#optimized-approach---sorting-and-merging)

---

## Naive Approach - Pairwise Merging

### Intuition:
The naive approach involves comparing each pair of intervals and merging them if they overlap. This is done repeatedly until no more intervals can be merged, i.e., all overlapping intervals are merged. While this approach guarantees correctness, it is inefficient because it requires repeated passes over the intervals.

### Steps:
1. Start with the list of intervals.
2. Use a flag to check if any merging happens in one pass.
3. During each pass, compare each pair of intervals.
4. If two intervals overlap, merge them into a single interval.
5. Continue until no more mergings happen in a pass.

### Code:
```go
func merge(intervals [][]int) [][]int {
    if len(intervals) <= 1 {
        return intervals
    }
    
    merged := intervals
    changed := true
    
    for changed {
        changed = false
        var newMerged [][]int
        skipNext := false
        
        for i := 0; i < len(merged); i++ {
            if skipNext {
                skipNext = false
                continue
            }
            
            if i < len(merged) - 1 && merged[i][1] >= merged[i+1][0] {
                // We found overlapping intervals, merge them
                newInterval := []int{
                    min(merged[i][0], merged[i+1][0]),
                    max(merged[i][1], merged[i+1][1]),
                }
                newMerged = append(newMerged, newInterval)
                skipNext = true
                changed = true
            } else {
                newMerged = append(newMerged, merged[i])
            }
        }
        merged = newMerged
    }
    
    return merged
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity:
- **Time Complexity:** O(N^2) in the worst case scenario, as we might have to keep combining intervals repeatedly.
- **Space Complexity:** O(N) where N is the number of intervals, as it uses an additional list to store merging intervals temporarily.

---

## Optimized Approach - Sorting and Merging

### Intuition:
This approach leverages sorting to simplify the problem. By sorting the intervals by their start times, we ensure that we're processing intervals in order. This allows us to merge intervals on a single pass through the sorted list, making the process much more efficient.

### Steps:
1. Sort the array based on the start times of the intervals.
2. Initialize an empty result list for storing merged intervals.
3. Traverse the sorted intervals. For each interval:
   - If the result list is empty or the last interval in the result does not overlap with the current interval, append it.
   - If they overlap, merge the current interval with the last interval in the result.
4. Continue this process until all intervals are processed.

### Code:
```go
import "sort"

func merge(intervals [][]int) [][]int {
    if len(intervals) <= 1 {
        return intervals
    }
    
    // Sort the intervals based on the start time
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })
    
    var mergedIntervals [][]int
    mergedIntervals = append(mergedIntervals, intervals[0])
    
    // Iterate over sorted intervals and merge if necessary
    for _, interval := range intervals[1:] {
        last := mergedIntervals[len(mergedIntervals) - 1]
        if last[1] >= interval[0] {
            // Overlapping, merge them
            mergedIntervals[len(mergedIntervals) - 1][1] = max(last[1], interval[1])
        } else {
            // Non-overlapping, just add the current interval
            mergedIntervals = append(mergedIntervals, interval)
        }
    }
    
    return mergedIntervals
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity:
- **Time Complexity:** O(N log N), due to the sorting step. The merging step is O(N).
- **Space Complexity:** O(N), used to store the result.

