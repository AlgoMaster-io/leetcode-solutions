# [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approach 1: Dynamic Programming with Sliding Window (Deque)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

import (
	"container/list"
)

type Solution struct{}

func (s *Solution) maxResult(nums []int, k int) int {
	deque := list.New() // Stores indices of potential max scores
	deque.PushBack(0)
	dp := make([]int, len(nums))
	dp[0] = nums[0]

	for i := 1; i < len(nums); i++ {
		// Remove indices that are out of the current window
		if deque.Len() > 0 && deque.Front().Value.(int) < i-k {
			deque.Remove(deque.Front())
		}

		// Update dp[i] using the max value in the deque
		dp[i] = nums[i] + dp[deque.Front().Value.(int)]

		// Maintain the deque to keep indices in decreasing order of their dp values
		for deque.Len() > 0 && dp[deque.Back().Value.(int)] <= dp[i] {
			deque.Remove(deque.Back())
		}

		deque.PushBack(i)
	}

	return dp[len(nums)-1]
}
```

## Approach 2: Priority Queue (Heap-Based)

### Solution
go
```go
// Time Complexity: O(n log k)
// Space Complexity: O(k)
package main

import (
	"container/heap"
)

type Solution struct{}

type Pair struct {
	score int
	index int
}

type MaxHeap []Pair

func (h MaxHeap) Len() int           { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i].score > h[j].score }
func (h MaxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MaxHeap) Push(x interface{}) {
	*h = append(*h, x.(Pair))
}

func (h *MaxHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func (s *Solution) maxResult(nums []int, k int) int {
	maxHeap := &MaxHeap{}
	heap.Init(maxHeap)
	heap.Push(maxHeap, Pair{score: nums[0], index: 0})
	score := nums[0]

	for i := 1; i < len(nums); i++ {
		// Remove elements outside the current window
		for (*maxHeap)[0].index < i-k {
			heap.Pop(maxHeap)
		}

		// The current score is the sum of the max score in the heap and nums[i]
		score = nums[i] + (*maxHeap)[0].score
		heap.Push(maxHeap, Pair{score: score, index: i})
	}

	return score
}
```

