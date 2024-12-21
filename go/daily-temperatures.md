# [Leetcode 739: Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Stack Approach](#stack-approach)

### Brute Force Approach

**Intuition**:  
The brute force approach involves checking each day to see when a warmer temperature will occur. For each day, we check all subsequent days to determine if the temperature is warmer. This approach uses a nested loop resulting in \(O(n^2)\) time complexity, which is not optimal but straightforward.

**Go Code**:
```go
func dailyTemperatures(T []int) []int {
    n := len(T)
    res := make([]int, n)
    
    // Iterate through each day
    for i := 0; i < n; i++ {
        // Check each subsequent day
        for j := i + 1; j < n; j++ {
            // If a warmer temperature is found
            if T[j] > T[i] {
                // Record the number of days
                res[i] = j - i
                break // No need to check further as we've found the next warmer day
            }
        }
    }
    
    return res
}
```

**Time Complexity**: \(O(n^2)\). This arises because for each element, we might end up looking through all remaining elements.
  
**Space Complexity**: \(O(1)\). We use a fixed amount of extra space aside from the result array.

### Stack Approach

**Intuition**:  
To improve the efficiency, we can use a stack that keeps track of the indices of the temperatures. The stack is used to find the next warmer temperature indices in a single pass through the temperatures list. We push an index onto the stack if it hasn't encountered a warmer temperature yet (useful for pending days). When we find a warmer temperature during our iteration, we resolve the number of waiting days based on stack entries.

**Go Code**:
```go
func dailyTemperatures(T []int) []int {
    n := len(T)
    res := make([]int, n)
    stack := []int{} // Stack to hold indices of the temperatures array
    
    // Iterate through the temperatures
    for i := 0; i < n; i++ {
        // As long as stack is not empty and the current temperature is greater than 
        // the temperature at the index at the top of the stack
        for len(stack) > 0 && T[i] > T[stack[len(stack)-1]] {
            index := stack[len(stack)-1] // Get the index of the top element in the stack
            stack = stack[:len(stack)-1] // Pop the top element
            res[index] = i - index // Calculate the number of days until a warmer temperature
        }
        stack = append(stack, i) // Push current day's index onto the stack
    }
    
    return res
}
```

**Time Complexity**: \(O(n)\). Each index is pushed and popped from the stack once.

**Space Complexity**: \(O(n)\). In the worst case, all indices could be pushed onto the stack.

The stack approach is more efficient and is a commonly used strategy for problems where the next greater element is needed. By processing each index only once, we reduce unnecessary comparisons that the brute force method does.

