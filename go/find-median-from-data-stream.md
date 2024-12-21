# [Leetcode 295: Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

In this problem, we need to design a data structure that supports the following operations:

1. `addNum(int num)`: Add a number to the data structure.
2. `findMedian()`: Return the median of all elements so far.

The challenge is to ensure that we can find the median quickly even as we add numbers to our data set.

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Using Two Heaps](#using-two-heaps)

## Brute Force Approach

### Intuition

A naive way to solve the problem is simply to keep a list where we append the numbers as they arrive and sort the list whenever we need to find the median.

By keeping a sorted order, the median can be found easily:
- If the list is odd, the median is the middle element.
- If the list is even, the median is the average of the two middle elements.

### Implementation

```go
type MedianFinder struct {
    nums []int
}

func Constructor() MedianFinder {
    return MedianFinder{nums: []int{}}
}

func (this *MedianFinder) AddNum(num int)  {
    this.nums = append(this.nums, num)     // Add the new number to the list
    sort.Ints(this.nums)                   // Sort the list to maintain ordered property
}

func (this *MedianFinder) FindMedian() float64 {
    n := len(this.nums)
    if n%2 == 1 {                          // If the list is of odd size, return middle element
        return float64(this.nums[n/2])
    }
    // If the list is of even size, return the average of the two middle elements
    return float64(this.nums[n/2-1] + this.nums[n/2]) / 2.0
}
```

### Complexity Analysis

- **Time Complexity**: 
  - `AddNum`: O(n log n) due to sorting.
  - `FindMedian`: O(1).

- **Space Complexity**: O(n) for storing all the numbers.

## Using Two Heaps

### Intuition

A more efficient solution involves using two heaps:
- A max heap to store the smaller half of the numbers.
- A min heap to store the larger half.

The key idea is to maintain balance between these two heaps such that the size difference is at most one. This allows us to find the median efficiently:
- If the total number of elements is odd, the median is the top element of the heap with more elements.
- If even, the median is the average of the tops of the two heaps.

### Implementation

```go
type MedianFinder struct {
    maxHeap *MaxHeap
    minHeap *MinHeap
}

func Constructor() MedianFinder {
    maxHeap := &MaxHeap{}   // Max-heap for the lower half of the numbers
    minHeap := &MinHeap{}   // Min-heap for the upper half of the numbers
    heap.Init(maxHeap)
    heap.Init(minHeap)
    return MedianFinder{maxHeap: maxHeap, minHeap: minHeap}
}

func (this *MedianFinder) AddNum(num int) {
    if this.maxHeap.Len() == 0 || num <= (*this.maxHeap)[0] {
        heap.Push(this.maxHeap, num)                     // Add to max heap
    } else {
        heap.Push(this.minHeap, num)                     // Add to min heap
    }

    // Balance the heaps: either both are the same size, or maxHeap has 1 more element
    if this.maxHeap.Len() > this.minHeap.Len()+1 {
        heap.Push(this.minHeap, heap.Pop(this.maxHeap))
    } else if this.minHeap.Len() > this.maxHeap.Len() {
        heap.Push(this.maxHeap, heap.Pop(this.minHeap))
    }
}

func (this *MedianFinder) FindMedian() float64 {
    if this.maxHeap.Len() > this.minHeap.Len() {
        return float64((*this.maxHeap)[0])               // Max heap has more elements
    }
    // Both heaps are of same size
    return float64((*this.maxHeap)[0]+(*this.minHeap)[0]) / 2.0
}

// MaxHeap definition (using container/heap)
type MaxHeap []int

func (h MaxHeap) Len() int           { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i] > h[j] } // Max-Heapify by changing comparison
func (h MaxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MaxHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *MaxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    item := old[n-1]
    *h = old[0 : n-1]
    return item
}

// MinHeap definition (using container/heap)
type MinHeap []int

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i] < h[j] } // Default Min-Heapify
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    item := old[n-1]
    *h = old[0 : n-1]
    return item
}
```

### Complexity Analysis

- **Time Complexity**:
  - `AddNum`: O(log n) for balancing the heaps.
  - `FindMedian`: O(1) because we just access the root of one or both heaps.

- **Space Complexity**: O(n) for storing the elements in the heaps.

This approach efficiently handles the median calculation in a dynamic data stream and balances the cost of both adding numbers and retrieving the median.

