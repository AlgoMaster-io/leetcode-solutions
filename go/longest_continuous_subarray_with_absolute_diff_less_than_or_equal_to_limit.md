# [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Approach 1: Sliding Window with Deque

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

import "container/list"

func longestSubarray(nums []int, limit int) int {
	maxDeque := list.New() // Stores indices of max elements in the window
	minDeque := list.New() // Stores indices of min elements in the window
	left, maxLength := 0, 0

	for right := 0; right < len(nums); right++ {
		// Maintain the decreasing order in maxDeque
		for maxDeque.Len() > 0 && nums[maxDeque.Back().Value.(int)] < nums[right] {
			maxDeque.Remove(maxDeque.Back())
		}
		maxDeque.PushBack(right)
		
		// Maintain the increasing order in minDeque
		for minDeque.Len() > 0 && nums[minDeque.Back().Value.(int)] > nums[right] {
			minDeque.Remove(minDeque.Back())
		}
		minDeque.PushBack(right)

		// Check if the current window is valid
		for nums[maxDeque.Front().Value.(int)] - nums[minDeque.Front().Value.(int)] > limit {
			left++ // Shrink the window from the left
			// Remove indices out of the window
			if maxDeque.Front().Value.(int) < left {
				maxDeque.Remove(maxDeque.Front())
			}
			if minDeque.Front().Value.(int) < left {
				minDeque.Remove(minDeque.Front())
			}
		}
		
		maxLength = Max(maxLength, right-left+1) // Update the max length
	}

	return maxLength
}

func Max(x, y int) int {
	if x > y {
		return x
	}
	return y
}
```

## Approach 2: Sliding Window with TreeMap

### Solution
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
package main

import (
	"sort"
)

type TreeMap struct {
	keys []int
	freq map[int]int
}

func NewTreeMap() *TreeMap {
	return &TreeMap{keys: []int{}, freq: make(map[int]int)}
}

func (tm *TreeMap) Increment(val int) {
	if _, exists := tm.freq[val]; !exists {
		tm.keys = append(tm.keys, val)
	}
	tm.freq[val]++
	sort.Ints(tm.keys)
}

func (tm *TreeMap) Decrement(val int) {
	if tm.freq[val] > 0 {
		tm.freq[val]--
		if tm.freq[val] == 0 {
			delete(tm.freq, val)
			for i, key := range tm.keys {
				if key == val {
					tm.keys = append(tm.keys[:i], tm.keys[i+1:]...)
					break
				}
			}
		}
	}
}

func (tm *TreeMap) Min() int {
	if len(tm.keys) > 0 {
		return tm.keys[0]
	}
	return 0
}

func (tm *TreeMap) Max() int {
	if len(tm.keys) > 0 {
		return tm.keys[len(tm.keys)-1]
	}
	return 0
}

func longestSubarrayWithTreeMap(nums []int, limit int) int {
	mapTM := NewTreeMap() // Stores elements and their frequencies
	left, maxLength := 0, 0

	for right := 0; right < len(nums); right++ {
		// Add the current element to the map
		mapTM.Increment(nums[right])

		// Shrink the window if the condition is violated
		for mapTM.Max()-mapTM.Min() > limit {
			mapTM.Decrement(nums[left])
			left++
		}

		maxLength = Max(maxLength, right-left+1) // Update the max length
	}

	return maxLength
}
```

