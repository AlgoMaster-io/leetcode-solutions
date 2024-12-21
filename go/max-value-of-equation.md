# [Leetcode 1499: Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized with Priority Queue](#approach-2-optimized-with-priority-queue)

## Approach 1: Brute Force

### Intuition
The equation to maximize is `yi + yj + |xi - xj|` where `xi < xj`. By manipulating, it can be rewritten as `yi + yj + xj - xi`, which leads us to `yi - xi + yj + xj`. For each pair of points (i, j) such that `xi < xj`, we compute this value and keep track of the maximum.

### Plan
- Iterate through each pair `(i, j)` such that `xi < xj`
- Compute the value of the equation
- Track the maximum value

### Code
```go
func findMaxValueOfEquation(points [][]int, k int) int {
    maxValue := math.MinInt64

    for i := 0; i < len(points); i++ {
        for j := i + 1; j < len(points); j++ {
            if points[j][0] - points[i][0] <= k {
                value := points[i][1] + points[j][1] + points[j][0] - points[i][0]
                if value > maxValue {
                    maxValue = value
                }
            }
        }
    }

    return maxValue
}
```

### Complexity Analysis
- **Time Complexity:** \(O(n^2)\), where \(n\) is the number of points, since we check each pair.
- **Space Complexity:** \(O(1)\) as we use only constant extra space.

## Approach 2: Optimized with Priority Queue

### Intuition
The equation can be rewritten to focus on maximizing `(yi - xi) + (yj + xj)`. This suggests a strategy of keeping track of `yi - xi` values with eligible `xi`. By utilizing a max heap (priority queue), we can efficiently track the maximum value of `yi - xi` within a valid range defined by `k`.

### Plan
- Use a max heap to store pairs of `(yi - xi, xi)` for eligible points:
  - Iterate through each point.
  - Remove points from the heap where the x-coordinate `xi` is too small (outside range determined by `k`). 
  - Calculate potential maximum value with top of the heap.
  - Push current `(yi - xi, xi)` into the heap.

### Code
```go
import (
    "container/heap"
    "math"
)

// Define a pair for max heap
type Pair struct {
    diff int
    x    int
}

// Implement a max heap
type MaxHeap []Pair

func (h MaxHeap) Len() int { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i].diff > h[j].diff }
func (h MaxHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(Pair)) }
func (h *MaxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func findMaxValueOfEquation(points [][]int, k int) int {
    maxHeap := &MaxHeap{}
    heap.Init(maxHeap)
    maxValue := math.MinInt64

    for _, point := range points {
        xj, yj := point[0], point[1]

        // Remove invalid points from heap (xi < xj and above k in distance)
        for maxHeap.Len() > 0 && xj - (*maxHeap)[0].x > k {
            heap.Pop(maxHeap)
        }

        // Calculate value using top of the heap
        if maxHeap.Len() > 0 {
            top := (*maxHeap)[0]
            maxValue = max(maxValue, top.diff + yj + xj)
        }

        // Push current point's (yj - xj, xj) into the heap
        heap.Push(maxHeap, Pair{yj - xj, xj})
    }

    return maxValue
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity Analysis
- **Time Complexity:** \(O(n \log n)\), due to heap operations for each of the \(n\) points.
- **Space Complexity:** \(O(n)\), for storing points in the heap.

