# 857. [Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approach: Sorting and Priority Queue (Max-Heap)

### Solution
go
```go
// Time Complexity: O(n * log(n) + n * log(k)), where n is the number of workers and k is the number of workers to hire
// Space Complexity: O(k), for the max-heap
import (
	"container/heap"
	"sort"
)

func mincostToHireWorkers(quality []int, wage []int, k int) float64 {
	n := len(quality)
	workers := make([][2]float64, n)

	// Create an array of workers with their wage-to-quality ratio and quality
	for i := 0; i < n; i++ {
		workers[i][0] = float64(wage[i]) / float64(quality[i]) // Wage-to-quality ratio
		workers[i][1] = float64(quality[i])
	}

	// Sort workers by their wage-to-quality ratio
	sort.Slice(workers, func(i, j int) bool {
		return workers[i][0] < workers[j][0]
	})

	maxHeap := &IntHeap{} // Max-heap for quality
	totalQuality := 0     // Sum of qualities in the current group
	minCost := float64(^uint(0) >> 1) // Initialize minCost to maximum possible value

	for _, worker := range workers {
		currentQuality := int(worker[1])
		totalQuality += currentQuality
		heap.Push(maxHeap, currentQuality)

		// If we exceed k workers, remove the one with the highest quality
		if maxHeap.Len() > k {
			totalQuality -= heap.Pop(maxHeap).(int)
		}

		// If we have exactly k workers, calculate the cost
		if maxHeap.Len() == k {
			minCost = min(minCost, float64(totalQuality)*worker[0])
		}
	}

	return minCost
}

// IntHeap is a max-heap of integers
type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] > h[j] } // Reverse comparison for max-heap
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

func min(a, b float64) float64 {
	if a < b {
		return a
	}
	return b
}
```

