# 215. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approach 1: Sorting

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(1)
import (
    "sort"
)

func findKthLargest(nums []int, k int) int {
    sort.Ints(nums) // Sort the array in ascending order
    return nums[len(nums)-k] // Return the kth largest element
}
```

## Approach 2: Using a Min-Heap

### Solution
go
```go
// Time Complexity: O(n log k)
// Space Complexity: O(k)
import (
    "container/heap"
)

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
        heap.Push(h, num)
        if h.Len() > k {
            heap.Pop(h)
        }
    }

    return (*h)[0] // The root of the heap is the kth largest element
}
```

## Approach 3: Quickselect (Partitioning)

### Solution
go
```go
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1)
import (
    "math/rand"
)

func findKthLargest(nums []int, k int) int {
    n := len(nums)
    return quickSelect(nums, 0, n-1, n-k)
}

func quickSelect(nums []int, left, right, k int) int {
    if left == right {
        return nums[left] // Base case: only one element
    }

    pivotIndex := left + rand.Intn(right-left+1)
    pivotIndex = partition(nums, left, right, pivotIndex)

    if pivotIndex == k {
        return nums[k] // Found the kth largest element
    } else if pivotIndex < k {
        return quickSelect(nums, pivotIndex+1, right, k) // Search in the right part
    } else {
        return quickSelect(nums, left, pivotIndex-1, k) // Search in the left part
    }
}

func partition(nums []int, left, right, pivotIndex int) int {
    pivotValue := nums[pivotIndex]
    nums[pivotIndex], nums[right] = nums[right], nums[pivotIndex] // Move pivot to the end
    storeIndex := left

    for i := left; i < right; i++ {
        if nums[i] < pivotValue {
            nums[storeIndex], nums[i] = nums[i], nums[storeIndex]
            storeIndex++
        }
    }

    nums[storeIndex], nums[right] = nums[right], nums[storeIndex] // Move pivot to its final position
    return storeIndex
}
```

