# [LeetCode 1014: Best Sightseeing Pair](https://leetcode.com/problems/best-sightseeing-pair/)

## Approaches
- [Naive Iterative Approach](#naive-iterative-approach)
- [Optimized Dynamic Approach](#optimized-dynamic-approach)

### Naive Iterative Approach

**Intuition**:  
In this approach, we evaluate the score for each potential pair of pairs `(i, j)` where `i < j` by calculating `A[i] + A[j] + i - j`. We iterate through all possible pairs, and compute this score to track the highest possible score. 

The naive solution essentially involves brute-forcing all possible pairs, which provides the correct result but can be inefficient for large arrays due to its O(n^2) time complexity. 

#### Time Complexity
- O(n^2) since we are using two nested loops to evaluate every pair.
  
#### Space Complexity
- O(1) as we do not use any additional data structures.

```go
func maxScoreSightseeingPair(A []int) int {
    n := len(A)
    maxScore := 0
    
    // Consider every possible pair (i, j) with i < j
    for i := 0; i < n-1; i++ {
        for j := i + 1; j < n; j++ {
            // Calculate the score for the pair (i, j)
            score := A[i] + A[j] + i - j
            // Update maxScore if this pair's score is the highest
            if score > maxScore {
                maxScore = score
            }
        }
    }
    
    return maxScore
}
```

### Optimized Dynamic Approach

**Intuition**:  
To solve the problem in a more optimal manner, we leverage the concept of separating the problem into two parts:  
1. Maximizing `A[i] + i`.  
2. Minimizing `j`.  

While iterating through the array, instead of recalculating potential maximums for every j, we keep a running maximum `max_i` which is the best option for `A[i] + i` encountered so far as we loop. 

For each element `j`, it suffices to compute `A[j] - j` while maintaining the optimal previous `max_i`. Thus, at each `j`, the best score considering all previous `i` is given by `max_i + A[j] - j`.

This approach significantly reduces the complexity as it leverages the ongoing maximum values efficiently.

#### Time Complexity
- O(n) since we traverse the array only once.

#### Space Complexity
- O(1) as we use constant extra space.

```go
func maxScoreSightseeingPair(A []int) int {
    maxScore := 0
    maxI := A[0] // This represents the best A[i] + i seen so far
    
    for j := 1; j < len(A); j++ {
        // Calculate current max score using maxI and A[j] - j
        maxScore = max(maxScore, maxI + A[j] - j)
        // Update maxI to be the maximum between the current maxI and A[j] + j
        maxI = max(maxI, A[j] + j)
    }
    
    return maxScore
}

// Utility function to get maximum of two integers
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

This optimized approach efficiently keeps track of the maximum scores by dynamically considering the best `A[i] + i` and updating through each `j`, ensuring we maintain an O(n) traversal.

