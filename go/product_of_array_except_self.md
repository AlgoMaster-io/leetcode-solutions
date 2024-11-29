# 238. [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Approach 1: Left and Right Product Arrays

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
func productExceptSelf(nums []int) []int {
    n := len(nums)
    left := make([]int, n)
    right := make([]int, n)
    result := make([]int, n)

    // Compute left products
    left[0] = 1
    for i := 1; i < n; i++ {
        left[i] = left[i-1] * nums[i-1]
    }

    // Compute right products
    right[n-1] = 1
    for i := n - 2; i >= 0; i-- {
        right[i] = right[i+1] * nums[i+1]
    }

    // Compute result by multiplying left and right products
    for i := 0; i < n; i++ {
        result[i] = left[i] * right[i]
    }

    return result
}
```

## Approach 2: Optimized Space with Single Array

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1) (ignoring output array)
func productExceptSelf(nums []int) []int {
    n := len(nums)
    result := make([]int, n)

    // Compute left products directly in result
    result[0] = 1
    for i := 1; i < n; i++ {
        result[i] = result[i-1] * nums[i-1]
    }

    // Compute right products and multiply with left products
    right := 1
    for i := n - 1; i >= 0; i-- {
        result[i] *= right
        right *= nums[i]
    }

    return result
}
```

