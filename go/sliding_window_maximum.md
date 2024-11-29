# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approach 1: Using Deque (Optimal Solution)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(k)
package main

import "container/list"

func maxSlidingWindow(nums []int, k int) []int {
    n := len(nums)
    result := make([]int, n-k+1)
    deque := list.New() // Store indices of elements

    for i := 0; i < n; i++ {
        // Remove indices out of the current window
        if deque.Len() > 0 && deque.Front().Value.(int) < i-k+1 {
            deque.Remove(deque.Front())
        }

        // Remove elements smaller than the current element from the back
        for deque.Len() > 0 && nums[deque.Back().Value.(int)] < nums[i] {
            deque.Remove(deque.Back())
        }

        deque.PushBack(i) // Add the current element's index to the deque

        // Add the maximum element of the current window to the result
        if i >= k-1 {
            result[i-k+1] = nums[deque.Front().Value.(int)]
        }
    }

    return result
}
```

## Approach 2: Using Priority Queue (Heap-Based)

### Solution
go
```go
// Time Complexity: O(n log k)
// Space Complexity: O(k)
package main

import (
    "container/heap"
)

func maxSlidingWindow(nums []int, k int) []int {
    n := len(nums)
    result := make([]int, n-k+1)
    maxHeap := &IntHeap{} // Max-heap: {value, index}

    for i := 0; i < n; i++ {
        // Add the current element to the heap
        heap.Push(maxHeap, [2]int{nums[i], i})

        // Remove elements that are out of the current window
        for maxHeap.Peek()[1] < i-k+1 {
            heap.Pop(maxHeap)
        }

        // Add the maximum element of the current window to the result
        if i >= k-1 {
            result[i-k+1] = maxHeap.Peek()[0]
        }
    }

    return result
}

// An IntHeap is a max-heap of int arrays.
type IntHeap [][2]int

func (h IntHeap) Len() int            { return len(h) }
func (h IntHeap) Less(i, j int) bool  { return h[i][0] > h[j][0] } // Greater than makes it a max-heap
func (h IntHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *IntHeap) Push(x interface{}) { *h = append(*h, x.([2]int)) }
func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
func (h *IntHeap) Peek() [2]int {
    return (*h)[0]
}
```

