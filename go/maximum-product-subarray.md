# [Leetcode 152: Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Solutions:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach using Dynamic Programming](#optimized-approach-using-dynamic-programming)

---

### Brute Force Approach

#### Intuition:
The brute force approach involves calculating the product of every possible subarray and updating the maximum product found. This method ensures that we explore all potential subarray combinations, but it is not efficient for large arrays due to its high time complexity.

#### Approach:
1. Iterate over all starting indices of the subarrays.
2. For each starting index, calculate the product of the subarray ending at each subsequent index.
3. Keep track of the maximum product of all subarrays calculated.

#### Code:
```go
func maxProduct(nums []int) int {
    maxProduct := nums[0] // Initialize maxProduct with the first element

    // Loop through all possible starting points of subarrays
    for i := 0; i < len(nums); i++ {
        currentProduct := 1
        // Calculate product for subarrays starting at `i`
        for j := i; j < len(nums); j++ {
            currentProduct *= nums[j]
            // Update maxProduct if we find a new maximum
            if currentProduct > maxProduct {
                maxProduct = currentProduct
            }
        }
    }
    return maxProduct
}
```

#### Time Complexity: 
- O(n^2) due to the nested loops.

#### Space Complexity: 
- O(1) since no additional space is used apart from variables.

---

### Optimized Approach using Dynamic Programming

#### Intuition:
The optimized solution leverages dynamic programming to track the maximum and minimum products up to the current position, considering that negative numbers and zero can drastically change the product direction (from negative to positive or to zero). By maintaining both, we can handle cases where a negative number flips the sign of the product or zero resets it.

#### Approach:
1. Initialize `maxProduct` with the first element.
2. Use two variables, `currentMax` and `currentMin`, to store the maximum and minimum product up to the current index.
3. Iterate through the array, updating `currentMax` and `currentMin` at each step:
   - The new `currentMax` is the maximum of:
     - The current number.
     - The product of `currentMax` and the current number.
     - The product of `currentMin` and the current number.
   - The new `currentMin` is similarly calculated as the minimum of the same values.
4. Update `maxProduct` with the current `currentMax` if it is greater.
5. Return the global `maxProduct`.

#### Code:
```go
func maxProduct(nums []int) int {
    if len(nums) == 0 {
        return 0
    }

    // Initialize maxProduct, currentMax, and currentMin with the first element
    maxProduct, currentMax, currentMin := nums[0], nums[0], nums[0]

    // Traverse through the array starting from the second element
    for i := 1; i < len(nums); i++ {
        num := nums[i]

        // When the number is negative, the currentMax and currentMin swap roles after multiplying
        if num < 0 {
            currentMax, currentMin = currentMin, currentMax
        }

        // Update currentMax and currentMin
        currentMax = max(num, currentMax*num)
        currentMin = min(num, currentMin*num)

        // Update maxProduct
        maxProduct = max(maxProduct, currentMax)
    }

    return maxProduct
}

// Helper function to get maximum of two integers
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

// Helper function to get minimum of two integers
func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

#### Time Complexity: 
- O(n) because we iterate through the array once.

#### Space Complexity: 
- O(1) since we use a constant amount of additional space.

---

