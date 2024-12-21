# [Leetcode 673: Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approaches
1. [Dynamic Programming with Recursion and Memoization](#approach1)
2. [Optimized Dynamic Programming](#approach2)

---

## Approach 1: Dynamic Programming with Recursion and Memoization

### Intuition:
As a starting approach, we aim to use recursion with memoization to find the longest increasing subsequences (LIS). For each element, we calculate the length of the longest subsequence ending at that element and count how many such subsequences exist. This involves recursively solving subproblems and storing results to avoid recomputation.

### Code:
```go
func findNumberOfLIS(nums []int) int {
    n := len(nums)
    if n == 0 {
        return 0
    }
    
    length := make([]int, n)  // Stores the LIS length ending at each index
    count := make([]int, n)   // Stores the count of LIS ending at each index
    for i := range length {
        length[i] = 1         // Each element alone is a subsequence of length 1
        count[i] = 1          // Initially, the count of such subsequence is 1
    }
    
    for i := 0; i < n; i++ {
        for j := 0; j < i; j++ {
            if nums[j] < nums[i] {
                if length[j]+1 > length[i] {
                    length[i] = length[j] + 1
                    count[i] = count[j]   // Reset the count to count[j]
                } else if length[j]+1 == length[i] {
                    count[i] += count[j] // Increment the count
                }
            }
        }
    }
    
    maxLength, numOfLIS := 0, 0
    for _, l := range length {
        if l > maxLength {
            maxLength = l
        }
    }

    // Sum up all the counts corresponding to maxLength
    for i := range nums {
        if length[i] == maxLength {
            numOfLIS += count[i]
        }
    }

    return numOfLIS
}
```
### Complexity:
- **Time Complexity:** \(O(n^2)\). Two nested loops are used to fill the length and count arrays.
- **Space Complexity:** \(O(n)\). We use additional arrays to store the length and count of LIS for each element.

---

## Approach 2: Optimized Dynamic Programming

### Intuition:
The recursive approach with memoization can be directly translated to a dynamic programming approach. The biggest observation here is to avoid recomputation using a table where, at each element's index, we store the maximum length of the increasing subsequence and the number of such sequences. This refined approach directly calculates these values in a more systematic manner without the overhead of recursion.

### Code:
```go
func findNumberOfLIS(nums []int) int {
    n := len(nums)
    if n <= 1 {
        return n
    }
    
    lengths := make([]int, n) // lengths[i] = longest ending in nums[i]
    counts := make([]int, n)  // count[i] = number of longest ending in nums[i]
    for i := 0; i < n; i++ {
        lengths[i] = 1
        counts[i] = 1
    }
    
    for j := 0; j < n; j++ {
        for i := 0; i < j; i++ {
            if nums[i] < nums[j] {
                if lengths[i] + 1 > lengths[j] {
                    lengths[j] = lengths[i] + 1
                    counts[j] = counts[i]
                } else if lengths[i] + 1 == lengths[j] {
                    counts[j] += counts[i]
                }
            }
        }
    }
    
    maxLen := 0
    for _, length := range lengths {
        if length > maxLen {
            maxLen = length
        }
    }
    
    result := 0
    for i := 0; i < n; i++ {
        if lengths[i] == maxLen {
            result += counts[i]
        }
    }
    
    return result
}
```
### Complexity:
- **Time Complexity:** \(O(n^2)\) as we need to traverse every possible pair once to fill out the length and count arrays.
- **Space Complexity:** \(O(n)\) due to the storage needed for the DP arrays `lengths` and `counts`.

This approach eliminates redundant calculations and leverages the power of dynamic programming to efficiently solve the problem.

