# 260. [Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approach 1: HashMap for Counting Frequencies

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func singleNumber(nums []int) []int {
    countMap := make(map[int]int)
    
    // Count the frequency of each number
    for _, num := range nums {
        countMap[num]++
    }
    
    result := make([]int, 0, 2)
    // Find the numbers with frequency 1
    for num, count := range countMap {
        if count == 1 {
            result = append(result, num)
        }
    }
    
    return result
}
```

## Approach 2: Bit Manipulation

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func singleNumber(nums []int) []int {
    xor := 0
    
    // XOR all numbers to get xor of the two unique numbers
    for _, num := range nums {
        xor ^= num
    }
    
    // Find a set bit (rightmost) in xor to separate the numbers
    diff := xor & -xor
    
    result := make([]int, 2)
    // Separate the numbers into two groups and XOR them respectively
    for _, num := range nums {
        if (num & diff) == 0 {
            result[0] ^= num // XOR for first group
        } else {
            result[1] ^= num // XOR for second group
        }
    }
    
    return result
}
```

