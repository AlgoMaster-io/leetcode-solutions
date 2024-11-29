# 480. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approach: Two Heaps (Max-Heap and Min-Heap)

### Solution
go
```go
// Time Complexity: O(n * log(k)), where n is the size of nums and k is the size of the window
// Space Complexity: O(k), for storing elements in the heaps
package main

import (
	"container/heap"
	"fmt"
)

// A MinHeap is a priority queue implementing heap.Interface using an array of int
type MinHeap []int

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
	*h = append(*h, x.(int))
}

func (h *MinHeap) Pop() interface{} {
	n := len(*h)
	x := (*h)[n-1]
	*h = (*h)[:n-1]
	return x
}

// A MaxHeap is a MinHeap with inverse comparison
type MaxHeap struct{ MinHeap }

func (h MaxHeap) Less(i, j int) bool { return h.MinHeap[i] > h.MinHeap[j] }

func medianSlidingWindow(nums []int, k int) []float64 {
	n := len(nums)
	result := make([]float64, n-k+1)

	maxHeap := &MaxHeap{} // Max-heap for the smaller half
	minHeap := &MinHeap{} // Min-heap for the larger half
	heap.Init(maxHeap)
	heap.Init(minHeap)

	for i := 0; i < n; i++ {
		// Add the new element into the appropriate heap
		if maxHeap.Len() == 0 || nums[i] <= maxHeap.MinHeap[0] {
			heap.Push(maxHeap, nums[i])
		} else {
			heap.Push(minHeap, nums[i])
		}

		// Rebalance the heaps if necessary
		if maxHeap.Len() > minHeap.Len()+1 {
			heap.Push(minHeap, heap.Pop(maxHeap))
		} else if minHeap.Len() > maxHeap.Len() {
			heap.Push(maxHeap, heap.Pop(minHeap))
		}

		// Remove the element that is sliding out of the window
		if i >= k {
			elementToRemove := nums[i-k]
			if elementToRemove <= maxHeap.MinHeap[0] {
				removeElement(maxHeap, elementToRemove)
			} else {
				removeElement(minHeap, elementToRemove)
			}

			// Rebalance the heaps after removal
			if maxHeap.Len() > minHeap.Len()+1 {
				heap.Push(minHeap, heap.Pop(maxHeap))
			} else if minHeap.Len() > maxHeap.Len() {
				heap.Push(maxHeap, heap.Pop(minHeap))
			}
		}

		// Add the median to the result when the window is fully formed
		if i >= k-1 {
			if maxHeap.Len() == minHeap.Len() {
				result[i-k+1] = (float64(maxHeap.MinHeap[0]) + float64(minHeap.MinHeap[0])) / 2
			} else {
				result[i-k+1] = float64(maxHeap.MinHeap[0])
			}
		}
	}

	return result
}

func removeElement(h heap.Interface, elem int) {
	for i := 0; i < h.Len(); i++ {
		if (*h.(*MinHeap))[i] == elem {
			heap.Remove(h, i)
			return
		}
	}
}

func main() {
	nums := []int{1, 3, -1, -3, 5, 3, 6, 7}
	k := 3
	fmt.Println(medianSlidingWindow(nums, k)) // Output: [1.00000,-1.00000,-1.00000,3.00000,5.00000,6.00000]
}
```

