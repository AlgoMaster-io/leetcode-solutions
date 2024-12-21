# [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approaches:

- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: Min-Heap](#approach-2-min-heap)
- [Approach 3: Quickselect (Optimized)](#approach-3-quickselect-optimized)

## Approach 1: Sorting

### Intuition:
The simplest approach to find the kth largest element in an array is to first sort the array in descending order. Once sorted, the kth largest element will simply be at the k-1 index due to zero-based indexing.

### Code:
```go
import "sort"

func findKthLargest(nums []int, k int) int {
    // Sort the array in ascending order
    sort.Ints(nums)
    // Access the element at the (len(nums) - k) position for the kth largest
    return nums[len(nums)-k]
}
```

### Complexity Analysis:
- **Time complexity**: O(n log n) due to sorting the array.
- **Space complexity**: O(1) if sorting can be done in-place; otherwise O(n) if extra space is needed for the sorted array.

## Approach 2: Min-Heap

### Intuition:
Use a min-heap to keep track of the k largest elements. When the size of the heap exceeds k, we remove the minimum. After iterating through the array, the top of the heap (the smallest in the heap) is the kth largest element.

### Code:
```go
import (
    "container/heap"
)

// A MinHeap struct that implements heap.Interface, it's a slice of integers.
type MinHeap []int

func (h MinHeap) Len() int            { return len(h) }
func (h MinHeap) Less(i, j int) bool  { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func findKthLargest(nums []int, k int) int {
    h := &MinHeap{}
    heap.Init(h)
    
    for _, num := range nums {
        // Push the current element onto the heap.
        heap.Push(h, num)
        // If heap length exceeds k, remove the smallest element (root of the heap)
        if h.Len() > k {
            heap.Pop(h)
        }
    }
    
    // The root of the heap is the kth largest element
    return heap.Pop(h).(int)
}
```

### Complexity Analysis:
- **Time complexity**: O(n log k), where n is the number of elements in the array. This is due to maintaining the heap of size k.
- **Space complexity**: O(k) due to storing k elements in the heap.

## Approach 3: Quickselect (Optimized)

### Intuition:
Quickselect is a selection algorithm that works similarly to QuickSort. Instead of sorting the entire array, Quickselect only partitions the array until the kth largest element is found, resulting in average-case linear time complexity.

### Code:
```go
import "math/rand"

func findKthLargest(nums []int, k int) int {
    // Convert kth largest to zero-based nth smallest
    kthSmallestIndex := len(nums) - k
    
    // Helper function to perform partitioning similar to QuickSort
    var partition func(nums []int, low, high int) int
    partition = func(nums []int, low, high int) int {
        // Choose a pivot randomly for average O(n) complexity
        pivotIndex := rand.Intn(high-low+1) + low
        pivotValue := nums[pivotIndex]
        // Move pivot to the end
        nums[pivotIndex], nums[high] = nums[high], nums[pivotIndex]
        
        storeIndex := low
        for i := low; i < high; i++ {
            if nums[i] < pivotValue {
                nums[i], nums[storeIndex] = nums[storeIndex], nums[i]
                storeIndex++
            }
        }
        nums[storeIndex], nums[high] = nums[high], nums[storeIndex]
        return storeIndex
    }
    
    var quickselect func(nums []int, low, high, k int) int
    quickselect = func(nums []int, low, high, k int) int {
        if low == high {
            return nums[low]
        }
        
        pivotIndex := partition(nums, low, high)
        
        if pivotIndex == k {
            return nums[k]
        } else if pivotIndex < k {
            return quickselect(nums, pivotIndex+1, high, k)
        } else {
            return quickselect(nums, low, pivotIndex-1, k)
        }
    }
    
    return quickselect(nums, 0, len(nums)-1, kthSmallestIndex)
}
```

### Complexity Analysis:
- **Time complexity**: O(n) average case, due to the partitioning step similar to QuickSort.
- **Space complexity**: O(1) additional space. In-place partitioning is used to rearrange elements within the array.

