# [1499. Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approach 1: Using a Priority Queue (Heap-Based)

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import (
    "container/heap"
)

type Item struct {
    value int // y - x
    x     int
}

type MaxHeap []Item

func (h MaxHeap) Len() int           { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i].value > h[j].value }
func (h MaxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MaxHeap) Push(x interface{}) {
    *h = append(*h, x.(Item))
}

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
    maxValue := int(^uint(0) >> 1) * -1 // Integer.MIN_VALUE

    for _, point := range points {
        x, y := point[0], point[1]

        // Remove points outside the range of k
        for maxHeap.Len() > 0 && x-(*maxHeap)[0].x > k {
            heap.Pop(maxHeap)
        }

        // If the heap is not empty, calculate the current value of the equation
        if maxHeap.Len() > 0 {
            maxValue = max(maxValue, (*maxHeap)[0].value+y+x)
        }

        // Add the current point to the heap
        heap.Push(maxHeap, Item{value: y - x, x: x})
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

## Approach 2: Using a Deque (Optimal Solution)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
type Pair struct {
    value int // y - x
    x     int
}

func findMaxValueOfEquation(points [][]int, k int) int {
    deque := []Pair{}
    maxValue := int(^uint(0) >> 1) * -1 // Integer.MIN_VALUE

    for _, point := range points {
        x, y := point[0], point[1]

        // Remove points outside the range of k
        for len(deque) > 0 && x-deque[0].x > k {
            deque = deque[1:]
        }

        // If the deque is not empty, calculate the current value of the equation
        if len(deque) > 0 {
            maxValue = max(maxValue, deque[0].value+y+x)
        }

        // Remove points from the back of the deque that have a smaller y - x value
        for len(deque) > 0 && deque[len(deque)-1].value <= y-x {
            deque = deque[:len(deque)-1]
        }

        // Add the current point to the deque
        deque = append(deque, Pair{value: y - x, x: x})
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

