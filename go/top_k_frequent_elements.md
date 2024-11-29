# 347. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approach 1: Min-Heap

### Solution
go
```go
// Time Complexity: O(n * log(k)), where n is the number of elements in nums
// Space Complexity: O(n), for the frequency map and heap
import (
    "container/heap"
)

type Element struct {
    value int
    count int
}

type MinHeap []Element

func (h MinHeap) Len() int { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].count < h[j].count }
func (h MinHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
    *h = append(*h, x.(Element))
}

func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func topKFrequent(nums []int, k int) []int {
    frequencyMap := make(map[int]int)
    for _, num := range nums {
        frequencyMap[num]++
    }

    minHeap := &MinHeap{}
    heap.Init(minHeap)

    for value, count := range frequencyMap {
        heap.Push(minHeap, Element{value: value, count: count})
        if minHeap.Len() > k {
            heap.Pop(minHeap)
        }
    }

    result := make([]int, k)
    for i := 0; i < k; i++ {
        result[i] = heap.Pop(minHeap).(Element).value
    }

    return result
}
```

## Approach 2: Bucket Sort

### Solution
go
```go
// Time Complexity: O(n), where n is the number of elements in nums
// Space Complexity: O(n), for the frequency map and buckets
func topKFrequent(nums []int, k int) []int {
    frequencyMap := make(map[int]int)
    for _, num := range nums {
        frequencyMap[num]++
    }

    buckets := make([][]int, len(nums)+1)
    for key, frequency := range frequencyMap {
        buckets[frequency] = append(buckets[frequency], key)
    }

    result := make([]int, 0, k)
    for i := len(buckets) - 1; i >= 0 && len(result) < k; i-- {
        if buckets[i] != nil {
            result = append(result, buckets[i]...)
        }
    }

    return result[:k]
}
```

