# [Leetcode 525: Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Prefix Sum Array](#approach-2-prefix-sum-array)
- [Approach 3: HashMap with Prefix Sum](#approach-3-hashmap-with-prefix-sum)

## Approach 1: Brute Force

### Intuition
The simplest approach to solving this problem is to explore all possible subarrays and check each one to see if it contains an equal number of 0s and 1s. This can be achieved by iterating over all possible start and end indices of the subarrays and counting the numbers of 0s and 1s.

### Steps
1. Iterate over all possible starting indices of subarrays.
2. For each start index, iterate over all possible ending indices.
3. For each subarray, calculate the number of 0s and 1s.
4. If they are equal, update the maximum length of such a subarray.

### Code
```go
func findMaxLength(nums []int) int {
    n := len(nums)
    maxLength := 0

    // Check each subarray by starting index i
    for i := 0; i < n; i++ {
        count1 := 0
        count0 := 0
        // Check the subarray ending index j
        for j := i; j < n; j++ {
            if nums[j] == 1 {
                count1++
            } else {
                count0++
            }
            // If counts of 0 and 1 are the same, update max length
            if count1 == count0 {
                maxLength = max(maxLength, j-i+1)
            }
        }
    }
    return maxLength
}

// max is a utility function to return the maximum of two integers
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity
- **Time Complexity**: O(n^2), where n is the number of elements in the array, because we explore every subarray.
- **Space Complexity**: O(1), only a few extra variables are used.

## Approach 2: Prefix Sum Array

### Intuition
The prefix sum approach involves calculating a running "balance" which is incremented for every `1` and decremented for every `0`. This approach transforms the problem into finding two indices with the same balance.

### Steps
1. Track a running balance, increase for `1` and decrease for `0`.
2. Use an array to store the first occurrence of each balance encountered.
3. Calculate the length of the subarray if the same balance has been seen before.

### Code
```go
func findMaxLength(nums []int) int {
    maxLength := 0
    balance := 0
    balanceIndex := make(map[int]int)
    balanceIndex[0] = -1 // Initialize to handle subarrays from the beginning

    for i, num := range nums {
        if num == 1 {
            balance++
        } else {
            balance--
        }
        
        if index, found := balanceIndex[balance]; found {
            // If balance is seen before, calculate max length
            maxLength = max(maxLength, i - index)
        } else {
            // Store the first occurrence of the balance
            balanceIndex[balance] = i
        }
    }

    return maxLength
}
```

### Complexity
- **Time Complexity**: O(n), because we traverse the array once.
- **Space Complexity**: O(n), for the hashmap that stores the balance indices.

## Approach 3: Optimized HashMap with Prefix Sum

### Intuition
While the previous solution is already optimal in both time and space, additional minor optimizations may include reducing map operations or memory usage. However, this solution is quite optimal in practical terms.

### Steps
- Similar to the previous approach, this utilizes running balance and the storage mechanism via a hashmap.
- Further optimizations may include reducing redundant checks but may not greatly change theoretical complexity.

The above provided hashmap solution from Approach 2 should be considered optimal for practical purposes.

