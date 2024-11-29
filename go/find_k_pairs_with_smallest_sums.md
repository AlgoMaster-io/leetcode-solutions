# 373. [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approach: Min-Heap

### Solution
go
```go
// Time Complexity: O(k * log(k)), where k is the number of pairs to find
// Space Complexity: O(k), for the heap
package main

import (
    "container/heap"
    "fmt"
)

type Pair struct {
    sum, num1, num2Index int
}

type MinHeap []Pair

func (h MinHeap) Len() int { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].sum < h[j].sum }
func (h MinHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
    *h = append(*h, x.(Pair))
}

func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func kSmallestPairs(nums1 []int, nums2 []int, k int) [][]int {
    minHeap := &MinHeap{}
    heap.Init(minHeap)
    
    // Initialize the heap with pairs (nums1[i], nums2[0]) for all i
    for i := 0; i < min(len(nums1), k); i++ {
        heap.Push(minHeap, Pair{nums1[i] + nums2[0], nums1[i], 0})
    }
    
    result := [][]int{}
    
    // Extract the smallest pairs until we have k pairs or the heap is empty
    for minHeap.Len() > 0 && k > 0 {
        current := heap.Pop(minHeap).(Pair)
        result = append(result, []int{current.num1, nums2[current.num2Index]})
        k--
        
        // If there are more pairs with the current nums1[i], add them to the heap
        if current.num2Index+1 < len(nums2) {
            nextPair := Pair{current.num1 + nums2[current.num2Index+1], current.num1, current.num2Index+1}
            heap.Push(minHeap, nextPair)
        }
    }
    
    return result
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func main() {
    nums1 := []int{1, 7, 11}
    nums2 := []int{2, 4, 6}
    k := 3
    result := kSmallestPairs(nums1, nums2, k)
    fmt.Println(result) // Output: [[1, 2], [1, 4], [1, 6]]
}
```

