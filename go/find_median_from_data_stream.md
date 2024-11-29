# 295. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approach: Two Heaps

### Solution
go
```go
// Time Complexity: O(log(n)) for adding a number, O(1) for finding the median
// Space Complexity: O(n), where n is the number of elements in the data stream
package main

import (
	"container/heap"
)

type MaxHeap []int
type MinHeap []int

func (h MaxHeap) Len() int           { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h MaxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MaxHeap) Push(x interface{}) {
	*h = append(*h, x.(int))
}

func (h *MaxHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
	*h = append(*h, x.(int))
}

func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

type MedianFinder struct {
	maxHeap *MaxHeap // To store the smaller half of the numbers
	minHeap *MinHeap // To store the larger half of the numbers
}

func Constructor() MedianFinder {
	maxHeap := &MaxHeap{}
	minHeap := &MinHeap{}
	heap.Init(maxHeap)
	heap.Init(minHeap)
	return MedianFinder{maxHeap, minHeap}
}

func (this *MedianFinder) AddNum(num int) {
	heap.Push(this.maxHeap, num)
	heap.Push(this.minHeap, heap.Pop(this.maxHeap))

	if this.maxHeap.Len() < this.minHeap.Len() {
		heap.Push(this.maxHeap, heap.Pop(this.minHeap))
	}
}

func (this *MedianFinder) FindMedian() float64 {
	if this.maxHeap.Len() > this.minHeap.Len() {
		return float64((*this.maxHeap)[0])
	}
	return (float64((*this.maxHeap)[0]) + float64((*this.minHeap)[0])) / 2.0
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AddNum(num);
 * param_2 := obj.FindMedian();
 */
```

