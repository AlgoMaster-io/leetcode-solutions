# 136. [Single Number](https://leetcode.com/problems/single-number/)

## Approach 1: Using HashMap

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func singleNumber(nums []int) int {
    countMap := make(map[int]int)

    // Count the frequency of each number
    for _, num := range nums {
        countMap[num]++
    }

    // Find the number with frequency 1
    for _, num := range nums {
        if countMap[num] == 1 {
            return num
        }
    }

    return -1 // No unique number found
}
```

## Approach 2: Using Sorting

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(1)
package main

import "sort"

func singleNumber(nums []int) int {
    sort.Ints(nums) // Sort the array

    // Traverse the array looking for unique element
    for i := 0; i < len(nums)-1; i += 2 {
        if nums[i] != nums[i+1] {
            return nums[i]
        }
    }

    return nums[len(nums)-1] // The last element is the single number
}
```

## Approach 3: Using XOR

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func singleNumber(nums []int) int {
    result := 0

    // XOR all elements, duplicate elements will cancel out
    for _, num := range nums {
        result ^= num
    }

    return result // The remaining single number
}
```

