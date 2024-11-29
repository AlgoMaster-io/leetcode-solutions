# 23. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approach 1: Priority Queue (Min-Heap)

### Solution
go
```go
// Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
// Space Complexity: O(k), for the priority queue
package main

import (
	"container/heap"
)

type ListNode struct {
	Val  int
	Next *ListNode
}

type MinHeap []*ListNode

func (h MinHeap) Len() int { return len(h) }
func (h MinHeap) Less(i, j int) bool {
	return h[i].Val < h[j].Val
}
func (h MinHeap) Swap(i, j int) {
	h[i], h[j] = h[j], h[i]
}
func (h *MinHeap) Push(x interface{}) {
	*h = append(*h, x.(*ListNode))
}
func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func mergeKLists(lists []*ListNode) *ListNode {
	minHeap := &MinHeap{}
	heap.Init(minHeap)

	// Add the head of each non-empty list to the heap
	for _, list := range lists {
		if list != nil {
			heap.Push(minHeap, list)
		}
	}

	dummy := &ListNode{}
	current := dummy

	for minHeap.Len() > 0 {
		// Get the smallest node from the heap
		smallest := heap.Pop(minHeap).(*ListNode)
		current.Next = smallest // Append it to the result
		current = current.Next

		// If the next node exists, add it to the heap
		if smallest.Next != nil {
			heap.Push(minHeap, smallest.Next)
		}
	}

	return dummy.Next // Return the merged list
}
```

## Approach 2: Divide and Conquer

### Solution
go
```go
// Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
// Space Complexity: O(log(k)), due to recursive calls
package main

type ListNode struct {
	Val  int
	Next *ListNode
}

func mergeKLists(lists []*ListNode) *ListNode {
	if lists == nil || len(lists) == 0 {
		return nil
	}
	return mergeLists(lists, 0, len(lists)-1)
}

func mergeLists(lists []*ListNode, left int, right int) *ListNode {
	if left == right {
		return lists[left]
	}

	mid := left + (right-left)/2
	l1 := mergeLists(lists, left, mid)
	l2 := mergeLists(lists, mid+1, right)
	return mergeTwoLists(l1, l2)
}

func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
	dummy := &ListNode{}
	current := dummy

	for l1 != nil && l2 != nil {
		if l1.Val < l2.Val {
			current.Next = l1
			l1 = l1.Next
		} else {
			current.Next = l2
			l2 = l2.Next
		}
		current = current.Next
	}

	if l1 != nil {
		current.Next = l1
	} else if l2 != nil {
		current.Next = l2
	}

	return dummy.Next
}
```

