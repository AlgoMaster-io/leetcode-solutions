# 973. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approach 1: Max-Heap

### Solution
go
```go
// Time Complexity: O(n * log(k)), where n is the number of points
// Space Complexity: O(k), for the max-heap
import (
	"container/heap"
	"math"
)

type PointHeap [][]int

func (h PointHeap) Len() int            { return len(h) }
func (h PointHeap) Less(i, j int) bool  { return distance(h[i]) > distance(h[j]) }
func (h PointHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }

func (h *PointHeap) Push(x interface{}) { *h = append(*h, x.([]int)) }
func (h *PointHeap) Pop() interface{} {
	n := len(*h)
	item := (*h)[n-1]
	*h = (*h)[:n-1]
	return item
}

func distance(point []int) int {
	return point[0]*point[0] + point[1]*point[1]
}

func kClosest(points [][]int, k int) [][]int {
	maxHeap := &PointHeap{}
	heap.Init(maxHeap)

	for _, point := range points {
		heap.Push(maxHeap, point)
		if maxHeap.Len() > k {
			heap.Pop(maxHeap) // Remove the farthest point
		}
	}

	result := make([][]int, k)
	for i := 0; i < k; i++ {
		result[i] = heap.Pop(maxHeap).([]int)
	}

	return result
}
```

## Approach 2: Quickselect

### Solution
go
```go
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1), as it modifies the input array in-place
import (
	"math/rand"
)

func kClosest(points [][]int, k int) [][]int {
	quickSelect(points, 0, len(points)-1, k)
	result := make([][]int, k)
	copy(result, points[:k])
	return result
}

func quickSelect(points [][]int, left, right, k int) {
	if left >= right {
		return
	}

	pivotIndex := partition(points, left, right)
	if pivotIndex == k {
		return
	} else if pivotIndex < k {
		quickSelect(points, pivotIndex+1, right, k)
	} else {
		quickSelect(points, left, pivotIndex-1, k)
	}
}

func partition(points [][]int, left, right int) int {
	pivot := points[right]
	pivotDistance := distance(pivot)
	i := left

	for j := left; j < right; j++ {
		if distance(points[j]) < pivotDistance {
			swap(points, i, j)
			i++
		}
	}
	swap(points, i, right)
	return i
}

func swap(points [][]int, i, j int) {
	points[i], points[j] = points[j], points[i]
}

func distance(point []int) int {
	return point[0]*point[0] + point[1]*point[1]
}
```

