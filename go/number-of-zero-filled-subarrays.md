# [LeetCode Problem 2348: Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)

## Table of Contents

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized Prefix-Sum](#approach-2-optimized-prefix-sum)
- [Approach 3: Two Pointers - Most Optimal](#approach-3-two-pointers-most-optimal)

---

## Approach 1: Brute Force

### Intuition

In this approach, we attempt to count all possible subarrays explicitly. For each subarray, check if it consists solely of zeros. This is a straightforward but inefficient method, as it involves checking each subarray one at a time.

### Implementation

```go
func zeroFilledSubarray(nums []int) int {
    count := 0
    n := len(nums)
    // Iterate over all possible starting points for subarrays
    for i := 0; i < n; i++ {
        // Find subarrays starting at index i
        for j := i; j < n; j++ {
            if nums[j] == 0 {
                count++
            } else {
                break // Exit early if a non-zero is found
            }
        }
    }
    return count
}
```

### Complexity Analysis

- **Time Complexity**: \(O(n^2)\) - We potentially examine each subarray.
- **Space Complexity**: \(O(1)\) - No additional space other than variables.

---

## Approach 2: Optimized Prefix-Sum

### Intuition

Instead of checking each subarray, we can take a more efficient approach by maintaining a running count of consecutive zeros using a prefix sum. Whenever we reach a non-zero, we can use combinatorial counting based on the length of the zero-filled segment we encountered.

### Implementation

```go
func zeroFilledSubarray(nums []int) int {
    count := 0
    currentStreak := 0
    // Iterate through the array
    for _, num := range nums {
        if num == 0 {
            // Increment current streak of zeros
            currentStreak++
            // Add the current streak count to total count
            count += currentStreak
        } else {
            // Reset streak when a non-zero is encountered
            currentStreak = 0
        }
    }
    return count
}
```

### Complexity Analysis

- **Time Complexity**: \(O(n)\) - We traverse the list once.
- **Space Complexity**: \(O(1)\) - Counts are kept within temporary variables.

---

## Approach 3: Two Pointers - Most Optimal

### Intuition

The optimal solution involves a two-pointer or sliding window technique, recognizing runs of zeros efficiently. By counting the length of each zero run in one pass through the array, we can calculate the number of subarrays at that point.

### Implementation

```go
func zeroFilledSubarray(nums []int) int {
    count, currentStreak := 0, 0
    // Traverse the array
    for _, num := range nums {
        if num == 0 {
            // Increase the streak
            currentStreak++
        } else {
            // Reset the streak
            currentStreak = 0
        }
        // Add number of subarrays formed by the current streak to count
        count += currentStreak
    }
    return count
}
```

### Complexity Analysis

- **Time Complexity**: \(O(n)\) - Similar to the prior optimal approach, traversing once through the array.
- **Space Complexity**: \(O(1)\) - Only uses a few integer variables.

This approach is generally preferred due to its simple logic and efficient handling of array segments in a single pass.

