# 1642. [Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approach 1: Max-Heap to Track Largest Jumps

### Solution
go
```go
// Time Complexity: O(n * log(k)), where n is the number of buildings and k is the number of jumps stored in the heap
// Space Complexity: O(k), where k is the size of the heap
package main

import (
    "container/heap"
)

type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *IntHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}
func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func furthestBuilding(heights []int, bricks int, ladders int) int {
    minHeap := &IntHeap{}
    heap.Init(minHeap) // Min-heap to store ladder-replaced climbs

    for i := 0; i < len(heights)-1; i++ {
        climb := heights[i+1] - heights[i]
        
        if climb > 0 {
            heap.Push(minHeap, climb) // Add the climb to the heap

            // If the number of climbs exceeds the number of ladders, use bricks for the smallest jump
            if minHeap.Len() > ladders {
                bricks -= heap.Pop(minHeap).(int)
            }

            // If bricks are exhausted, return the current building index
            if bricks < 0 {
                return i
            }
        }
    }

    return len(heights) - 1 // If all buildings are reachable
}
```

