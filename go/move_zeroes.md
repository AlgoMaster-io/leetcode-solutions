# [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

func moveZeroes(nums []int) {
    // Loop through the array
    for i := 0; i < len(nums); i++ {
        if nums[i] == 0 {
            // Find the next non-zero element and swap
            for j := i + 1; j < len(nums); j++ {
                if nums[j] != 0 {
                    // Swap the elements
                    nums[i], nums[j] = nums[j], nums[i]
                    break
                }
            }
        }
    }
}
```

## Approach 2: Two-Pointer Technique

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func moveZeroes(nums []int) {
    lastNonZeroFoundAt := 0 // Index to place the next non-zero element
    
    // Move all non-zero elements to the front of the array
    for i := 0; i < len(nums); i++ {
        if nums[i] != 0 {
            nums[lastNonZeroFoundAt] = nums[i]
            lastNonZeroFoundAt++
        }
    }
    
    // Fill the remaining positions with zeros
    for i := lastNonZeroFoundAt; i < len(nums); i++ {
        nums[i] = 0
    }
}
```

## Approach 3: Optimal Two-Pointer with Swapping

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func moveZeroes(nums []int) {
    left := 0 // Pointer to track the position of the next non-zero element
    
    // Iterate through the array
    for right := 0; right < len(nums); right++ {
        if nums[right] != 0 {
            // Swap the non-zero element with the left pointer
            nums[left], nums[right] = nums[right], nums[left]
            left++ // Increment the left pointer
        }
    }
}
```

