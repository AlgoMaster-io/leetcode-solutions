# [Leetcode 1851: Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)

## Approaches
1. [Brute Force](#brute-force)
2. [Optimized with Sorting and Priority Queue](#optimized-with-sorting-and-priority-queue)

## Brute Force

### Intuition
The brute force approach involves iterating through each query and for each query checking all intervals to find the smallest interval that can include the query. This approach is straightforward but not efficient due to the large number of intervals and queries.

### Approach
- For each query, check all intervals to see which intervals can include the query.
- Track the size of each valid interval that includes the query.
- Return the smallest such valid interval size for each query.

### Code

```go
func minInterval(intervals [][]int, queries []int) []int {
    // Result array to hold the size of the minimum interval for each query
    res := make([]int, len(queries))
    for i := 0; i < len(queries); i++ {
        minSize := -1
        for _, interval := range intervals {
            start, end := interval[0], interval[1]
            if queries[i] >= start && queries[i] <= end {
                size := end - start + 1
                if minSize == -1 || size < minSize {
                    minSize = size
                }
            }
        }
        res[i] = minSize
    }
    return res
}
```

### Complexity
- **Time Complexity:** O(n * m), where n is the number of queries and m is the number of intervals.
- **Space Complexity:** O(1), except for the result which is O(n).

## Optimized with Sorting and Priority Queue

### Intuition
The optimal approach takes advantage of sorting and a priority queue (min-heap). By processing queries in an increasing order and using a heap to manage overlapping intervals, we can efficiently determine the minimum interval for each query.

### Approach
1. Sort intervals by their starting points.
2. Sort the queries, keeping track of their original indices.
3. Use a min-heap to manage the current active intervals for each query.
4. For each query, add all intervals that start before or at the query point to the heap.
5. Remove intervals from the heap that end before the query point.
6. The top of the heap gives us the smallest interval that can contain the query.
7. Store the result using the original index of the query.

### Code

```go
import (
    "container/heap"
    "sort"
)

// Heap to store intervals; implements heap.Interface
type IntervalHeap [][2]int

func (h IntervalHeap) Len() int           { return len(h) }
func (h IntervalHeap) Less(i, j int) bool { return h[i][1] < h[j][1] } // Compare by interval size
func (h IntervalHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntervalHeap) Push(x interface{}) {
    *h = append(*h, x.([2]int))
}

func (h *IntervalHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func minInterval(intervals [][]int, queries []int) []int {
    // Sort intervals by their start time
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })
    
    // Map to track original indices of queries
    originalIdx := make(map[int]int)
    for i, query := range queries {
        originalIdx[query] = i
    }
    
    // Sort queries
    sort.Ints(queries)
    
    res := make([]int, len(queries))
    intervalHeap := &IntervalHeap{}
    heap.Init(intervalHeap)
    i := 0
    
    for _, query := range queries {
        // Add eligible intervals to the heap
        for i < len(intervals) && intervals[i][0] <= query {
            start, end := intervals[i][0], intervals[i][1]
            // Only add intervals that could possibly contain the query
            if end >= query {
                heap.Push(intervalHeap, [2]int{start, end - start + 1})
            }
            i++
        }
        
        // Remove intervals that cannot contain the query
        for intervalHeap.Len() > 0 && (*intervalHeap)[0][0] > query {
            heap.Pop(intervalHeap)
        }
        
        // Retrieve the smallest interval from the heap
        if intervalHeap.Len() > 0 {
            res[originalIdx[query]] = (*intervalHeap)[0][1]
        } else {
            res[originalIdx[query]] = -1
        }
    }
    
    return res
}
```

### Complexity
- **Time Complexity:** O((n + m) log m), where n is the number of queries and m is the number of intervals. Sorting takes O(m log m) and adding/removing from the heap in O(log m).
- **Space Complexity:** O(m) for the heap in the worst case. 

This enhanced solution efficiently handles large inputs by leveraging sorting and a priority queue to manage intervals dynamically as queries progress.

