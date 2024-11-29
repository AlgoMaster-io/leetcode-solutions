# [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
func MaxProduct(nums []int) int {
    maxProduct := int(^uint(0) >> 1) * -1 // Initialize to minimum integer value

    // Iterate through all possible subarrays
    for i := 0; i < len(nums); i++ {
        currentProduct := 1

        for j := i; j < len(nums); j++ {
            currentProduct *= nums[j]
            if currentProduct > maxProduct {
                maxProduct = currentProduct
            }
        }
    }

    return maxProduct
}
```

## Approach 2: Kadane's algorithm with Min/Max Tracking (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func MaxProduct(nums []int) int {
    maxProduct := nums[0]
    currentMax := nums[0]
    currentMin := nums[0]

    // Traverse the array
    for i := 1; i < len(nums); i++ {
        num := nums[i]

        // Swap currentMax and currentMin if num is negative
        if num < 0 {
            currentMax, currentMin = currentMin, currentMax
        }

        // Update currentMax and currentMin
        currentMax = Max(num, currentMax*num)
        currentMin = Min(num, currentMin*num)

        // Update the global maximum product
        maxProduct = Max(maxProduct, currentMax)
    }

    return maxProduct
}

func Max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func Min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

