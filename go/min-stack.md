# [Leetcode 155: Min Stack](https://leetcode.com/problems/min-stack/)

## Approaches

1. [Basic Approach: Two Separate Stacks](#basic-approach-two-separate-stacks)
2. [Optimized Approach: One Stack with Pairing](#optimized-approach-one-stack-with-pairing)

### Basic Approach: Two Separate Stacks

**Intuition:**

The most straightforward way to tackle the Min Stack problem is to use two distinct stacks:

1. **Main Stack**: This maintains the usual stack operations (`push`, `pop`, `top`).
2. **Minimum Stack**: This auxiliary stack keeps track of the minimum values.

When a value is pushed onto the main stack, we compare it with the current minimum (top of the min stack) and push the minimum of the two onto the min stack. The min stack thus always reflects the current minimum element of the main stack at any point in time.

**Algorithm:**

- For `push`: Push the element to the main stack. If the min stack is empty or the element is less than or equal to the top of the min stack, push it onto the min stack.
- For `pop`: Pop the element from the main stack. If the popped element is equal to the top of the min stack, pop from the min stack as well.
- For `top`: Return the top of the main stack.
- For `getMin`: Return the top of the min stack, which always holds the minimum value of the main stack.

**Time Complexity:** O(1) for each operation.

**Space Complexity:** O(n) for maintaining two stacks of the same size, where n is the number of operations.

```go
type MinStack struct {
    stack    []int
    minStack []int
}

func Constructor() MinStack {
    return MinStack{}
}

func (s *MinStack) Push(val int) {
    s.stack = append(s.stack, val)
    if len(s.minStack) == 0 || val <= s.minStack[len(s.minStack)-1] {
        s.minStack = append(s.minStack, val)
    }
}

func (s *MinStack) Pop() {
    if len(s.stack) == 0 {
        return
    }
    if s.stack[len(s.stack)-1] == s.minStack[len(s.minStack)-1] {
        s.minStack = s.minStack[:len(s.minStack)-1]
    }
    s.stack = s.stack[:len(s.stack)-1]
}

func (s *MinStack) Top() int {
    return s.stack[len(s.stack)-1]
}

func (s *MinStack) GetMin() int {
    return s.minStack[len(s.minStack)-1]
}
```

### Optimized Approach: One Stack with Pairing

**Intuition:**

In the optimized approach, we use a single stack but store pairs (or tuples) containing the value and the current minimum. Thus, each time we `push`, we store both the value and the minimum value up to that point.

- This approach reduces the space use required for maintaining a separate min stack by integrating the minimum calculations into the main stack itself.

**Algorithm:**

- For `push`: Push a pair (current value, current minimum) onto the stack.
- For `pop`: Pop from the stack, which includes both the value and current minimum.
- For `top`: Access the value of the top element in the stack.
- For `getMin`: Access the minimum value of the top element in the stack, ensuring constant time retrieval.

**Time Complexity:** O(1) for each operation.

**Space Complexity:** O(n), as the stack grows with each operation but now integrates the minimum information.

```go
type MinStack struct {
    stack []struct {
        value int
        min   int
    }
}

func Constructor() MinStack {
    return MinStack{}
}

func (s *MinStack) Push(val int) {
    min := val
    if len(s.stack) > 0 && s.stack[len(s.stack)-1].min < val {
        min = s.stack[len(s.stack)-1].min
    }
    s.stack = append(s.stack, struct{ value, min int }{val, min})
}

func (s *MinStack) Pop() {
    if len(s.stack) > 0 {
        s.stack = s.stack[:len(s.stack)-1]
    }
}

func (s *MinStack) Top() int {
    return s.stack[len(s.stack)-1].value
}

func (s *MinStack) GetMin() int {
    return s.stack[len(s.stack)-1].min
}
```

Both approaches guarantee average constant time complexity for each stack operation while maintaining the stack's integrity with respect to minimum value retrieval. The optimized single-stack approach, while still O(n) in space, can be more elegant and concise.

