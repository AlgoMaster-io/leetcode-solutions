# [155. Min Stack](https://leetcode.com/problems/min-stack/)

## Approach: Two Stacks for Tracking Minimum

### Solution
go
```go
// Time Complexity:
// - push: O(1)
// - pop: O(1)
// - top: O(1)
// - getMin: O(1)
// Space Complexity: O(n)

type MinStack struct {
	stack    []int
	minStack []int
}

func Constructor() MinStack {
	return MinStack{
		stack:    []int{},
		minStack: []int{},
	}
}

func (this *MinStack) Push(val int) {
	// Push the value onto the main stack
	this.stack = append(this.stack, val)

	// Push the new minimum onto the minStack
	if len(this.minStack) == 0 || val <= this.minStack[len(this.minStack)-1] {
		this.minStack = append(this.minStack, val)
	}
}

func (this *MinStack) Pop() {
	// If the top element of stack is the current minimum, pop it from minStack too
	if this.stack[len(this.stack)-1] == this.minStack[len(this.minStack)-1] {
		this.minStack = this.minStack[:len(this.minStack)-1]
	}
	this.stack = this.stack[:len(this.stack)-1]
}

func (this *MinStack) Top() int {
	return this.stack[len(this.stack)-1]
}

func (this *MinStack) GetMin() int {
	return this.minStack[len(this.minStack)-1]
}
```

