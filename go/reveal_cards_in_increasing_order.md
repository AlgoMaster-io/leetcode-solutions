# 950. [Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Approach 1: Simulation Using Deque

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
package main

import (
	"container/deque"
	"sort"
)

func deckRevealedIncreasing(deck []int) []int {
	n := len(deck)
	result := make([]int, n)
	sort.Ints(deck) // Sort the deck in increasing order

	dq := deque.New()

	// Add indices to the deque in order
	for i := 0; i < n; i++ {
		dq.PushBack(i)
	}

	// Place the smallest cards in the positions determined by deque
	for _, card := range deck {
		result[dq.PopFront().(int)] = card // Place the current smallest card
		if dq.Len() > 0 {
			dq.PushBack(dq.PopFront()) // Move the next index to the back
		}
	}

	return result
}
```

## Approach 2: Simulation Using Queue

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
package main

import (
	"container/list"
	"sort"
)

func deckRevealedIncreasing(deck []int) []int {
	sort.Ints(deck) // Sort the deck in increasing order
	queue := list.New()

	// Add indices to the queue in order
	for i := 0; i < len(deck); i++ {
		queue.PushBack(i)
	}

	result := make([]int, len(deck))

	// Place the smallest cards in the positions determined by the queue
	for _, card := range deck {
		result[queue.Remove(queue.Front()).(int)] = card // Place the current smallest card
		if queue.Len() > 0 {
			queue.PushBack(queue.Remove(queue.Front())) // Move the next index to the back
		}
	}

	return result
}
```

