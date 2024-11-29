# 703. [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approach: Min-Heap

### Solution
go
```go
// Time Complexity: O(n * log(k)) for initialization and O(log(k)) for each add operation
// Space Complexity: O(k), where k is the size of the heap
package main

import (
	"container/heap"
)

// An IntHeap is a min-heap of integers.
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

type KthLargest struct {
	minHeap *IntHeap
	k       int
}

func Constructor(k int, nums []int) KthLargest {
	minHeap := &IntHeap{}
	heap.Init(minHeap)

	kthLargest := KthLargest{
		minHeap: minHeap,
		k:       k,
	}

	// Add all elements to the heap and maintain only k elements
	for _, num := range nums {
		kthLargest.Add(num)
	}

	return kthLargest
}

func (this *KthLargest) Add(val int) int {
	heap.Push(this.minHeap, val)

	// Remove the smallest element if the heap exceeds size k
	if this.minHeap.Len() > this.k {
		heap.Pop(this.minHeap)
	}

	// The kth largest element is the root of the heap
	return (*this.minHeap)[0]
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * obj := Constructor(k, nums);
 * param_1 := obj.Add(val);
 */
```

