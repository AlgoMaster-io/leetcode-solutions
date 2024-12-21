# [Leetcode 2461: Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

## Table of contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with HashSet](#approach-2-sliding-window-with-hashset)

## Approach 1: Brute Force

### Intuition
The most straightforward approach to solving this problem is to check all possible subarrays of length `k` in the input array. For each subarray, check if all the elements are distinct, and if they are, calculate the sum. Keep track of the maximum sum found this way.

### Steps
1. Iterate over the array, considering each subarray of length `k`.
2. For each subarray, use a set to check if all elements are distinct.
3. If they are distinct, compute the sum and update the maximum sum if it's larger than the previously recorded maximum.
4. Return the maximum sum encountered.

### Time Complexity
- O(n * k), where `n` is the length of the array.

### Space Complexity
- O(k) for the set that stores the elements of the subarray.

```go
func maxSumOfDistinctSubarrays(arr []int, k int) int {
    maxSum := 0
    
    for i := 0; i <= len(arr) - k; i++ {
        unique := true
        currentSet := make(map[int]struct{})
        currentSum := 0
        
        for j := 0; j < k; j++ {
            if _, exists := currentSet[arr[i+j]]; exists {
                unique = false
                break
            }
            currentSet[arr[i+j]] = struct{}{}
            currentSum += arr[i+j]
        }
        
        if unique && currentSum > maxSum {
            maxSum = currentSum
        }
    }
    
    return maxSum
}
```

## Approach 2: Sliding Window with HashSet

### Intuition
To optimize the solution, we can use a sliding window approach combined with a hash set to keep track of distinct elements within the window. By maintaining a window of size `k`, we can efficiently check for distinct elements and compute the maximum sum.

### Steps
1. Initialize a window of size `k` with the first `k` elements, using a hash set to ensure all elements are distinct.
2. If they are not distinct, move the window by removing the first element and adding the next element, continuing this process.
3. Calculate the sum of each valid window and track the maximum sum found.
4. Continue moving the window across the array until reaching the end.
5. Return the maximum sum.

### Time Complexity
- O(n), as each element is added and removed from the hash set at most once.

### Space Complexity
- O(k), for storing the elements of the current window in the hash set.

```go
func maxSumOfDistinctSubarrays(arr []int, k int) int {
    if len(arr) < k {
        return 0
    }
    
    currentSet := make(map[int]struct{})
    sum := 0
    maxSum := 0
    
    // Initialize the first window
    for i := 0; i < k; i++ {
        if _, exists := currentSet[arr[i]]; exists {
            return 0 // If there are duplicates in the initial window
        }
        currentSet[arr[i]] = struct{}{}
        sum += arr[i]
    }
    
    maxSum = sum
    
    // Slide the window across the rest of the array
    for end := k; end < len(arr); end++ {
        start := end - k
        
        // Remove the element going out of the window
        delete(currentSet, arr[start])
        sum -= arr[start]
        
        // Add the new element
        if _, exists := currentSet[arr[end]]; exists { // Check if element is unique in the window
            continue
        }
        currentSet[arr[end]] = struct{}{}
        sum += arr[end]
        
        // Update max sum if the window is valid (all elements are distinct)
        if len(currentSet) == k {
            if sum > maxSum {
                maxSum = sum
            }
        }
    }
    
    return maxSum
}
```

This version implements an efficient sliding window mechanism keeping time complexity minimal while ensuring that the elements within each window are distinct using a hash set.

