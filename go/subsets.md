# 78. [Subsets](https://leetcode.com/problems/subsets/)

## Approach 1: Iterative (Power Set)

### Solution
go
```go
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n * 2^n) for storing the subsets
package main

func subsets(nums []int) [][]int {
    result := [][]int{{}} // start with the empty subset

    for _, num := range nums {
        size := len(result)
        for i := 0; i < size; i++ {
            subset := append([]int{}, result[i]...) // Create a copy of the existing subset
            subset = append(subset, num)            // Add the current number to each existing subset
            result = append(result, subset)
        }
    }

    return result
}
```

## Approach 2: Backtracking (Recursive Subset Generation)

### Solution
go
```go
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n) for the recursion stack
package main

func subsets(nums []int) [][]int {
    result := [][]int{}
    backtrack(nums, 0, []int{}, &result)
    return result
}

func backtrack(nums []int, index int, current []int, result *[][]int) {
    currCopy := append([]int{}, current...) // Ensure a copy of current
    *result = append(*result, currCopy)

    for i := index; i < len(nums); i++ {
        current = append(current, nums[i])      // Include nums[i] in the current subset
        backtrack(nums, i+1, current, result)   // Recurse with the next index
        current = current[:len(current)-1]      // Backtrack to explore other subsets
    }
}
```

## Approach 3: Bitmasking (Iterative Subset Generation)

### Solution
go
```go
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n * 2^n) for storing the subsets
package main

func subsets(nums []int) [][]int {
    result := [][]int{}
    totalSubsets := 1 << len(nums) // Total subsets = 2^n

    for mask := 0; mask < totalSubsets; mask++ {
        subset := []int{}
        for i := 0; i < len(nums); i++ {
            if (mask & (1 << i)) != 0 {
                subset = append(subset, nums[i]) // Include nums[i] in the subset
            }
        }
        result = append(result, subset)
    }

    return result
}
```

