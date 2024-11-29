# 525. [Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approach 1: Brute Force (Basic)

### Solution
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
func findMaxLength(nums []int) int {
    maxLength := 0
    
    // Check all possible subarrays
    for i := 0; i < len(nums); i++ {
        count := 0
        
        for j := i; j < len(nums); j++ {
            // Increment or decrement count based on value
            if nums[j] == 1 {
                count++
            } else {
                count--
            }

            // If count is zero, it means equal number of 0s and 1s
            if count == 0 {
                maxLength = max(maxLength, j-i+1)
            }
        }
    }
    
    return maxLength
}

// Helper function to get max value
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: HashMap (Optimal)

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
func findMaxLength(nums []int) int {
    mapIdx := make(map[int]int)
    mapIdx[0] = -1 // Initialize with a count of 0 at index -1
    maxLength := 0
    count := 0

    for i := 0; i < len(nums); i++ {
        // Increment or decrement count based on value
        if nums[i] == 1 {
            count++
        } else {
            count--
        }

        if idx, exists := mapIdx[count]; exists {
            // Calculate the length of the subarray
            maxLength = max(maxLength, i-idx)
        } else {
            // Store the first occurrence of this count
            mapIdx[count] = i
        }
    }

    return maxLength
}
```

