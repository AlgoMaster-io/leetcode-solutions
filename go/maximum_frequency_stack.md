# 895. [Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)

## Approach: HashMap with Stack for Frequency Tracking

### Solution
go
```go
// Time Complexity:
//   - push: O(1)
//   - pop: O(1)
// Space Complexity: O(n), where n is the number of elements in the stack
package main

type FreqStack struct {
	freqMap   map[int]int         // Maps element to its frequency
	groupMap  map[int][]int       // Maps frequency to a list (as a stack) of elements
	maxFreq   int                 // Tracks the maximum frequency
}

func Constructor() FreqStack {
	return FreqStack{
		freqMap:  make(map[int]int),
		groupMap: make(map[int][]int),
		maxFreq:  0,
	}
}

func (this *FreqStack) Push(val int) {
	// Update frequency of the element
	freq := 1 + this.freqMap[val]
	this.freqMap[val] = freq

	// Update max frequency
	if freq > this.maxFreq {
		this.maxFreq = freq
	}

	// Add the element to the stack corresponding to its frequency
	this.groupMap[freq] = append(this.groupMap[freq], val)
}

func (this *FreqStack) Pop() int {
	// Pop the most frequent element
	group := this.groupMap[this.maxFreq]
	val := group[len(group)-1]
	this.groupMap[this.maxFreq] = group[:len(group)-1]

	// Decrement the frequency of the popped element
	this.freqMap[val]--

	// If no elements are left at the current max frequency, decrement maxFreq
	if len(this.groupMap[this.maxFreq]) == 0 {
		this.maxFreq--
	}

	return val
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(val);
 * param_2 := obj.Pop();
 */
```

