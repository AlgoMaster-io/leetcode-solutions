# [Leetcode 1937: Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost/)

## Approaches
- [Approach 1: Dynamic Programming with Brute Force Transition](#approach-1)
- [Approach 2: Optimized Dynamic Programming with Two Auxiliary Arrays](#approach-2)
- [Approach 3: Optimized Dynamic Programming with Prefix and Suffix Arrays](#approach-3)

## Approach 1: Dynamic Programming with Brute Force Transition

### Intuition:
The problem involves choosing columns in a 2D array such that moving between columns incurs a certain cost. We aim to maximize the sum of points after considering these costs while moving from one row to the next.

1. Use a dynamic programming (DP) approach to maximize the scores while traversing from top to bottom.
2. Let `dp[i][j]` be the maximum score obtainable if you choose the `j`th column in the `i`th row.
3. Compute the DP transitions by checking each column in the previous row to determine the maximum score for each column in the current row.

### Time Complexity:
- O(m * m * n) where m is the number of rows and n is the number of columns. This complexity arises because for each element, we consider all elements from the previous row to calculate the maximum.

### Space Complexity:
- O(m * n) due to storing the scores in a DP table.

```go
func maxPoints(points [][]int) int {
    m, n := len(points), len(points[0])
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }
    
    // Base case: the first row is itself
    for j := 0; j < n; j++ {
        dp[0][j] = points[0][j]
    }
    
    // Fill the DP table
    for i := 1; i < m; i++ {
        for j := 0; j < n; j++ {
            maxPrev := 0
            for k := 0; k < n; k++ {
                // Calculate max for cell (i, j) considering all columns `k` from previous row
                val = dp[i-1][k] - abs(j-k)
                maxPrev = max(maxPrev, val)
            }
            dp[i][j] = points[i][j] + maxPrev
        }
    }
    
    // Find the maximum score in the last row
    result := 0
    for j := 0; j < n; j++ {
        result = max(result, dp[m-1][j])
    }
    return result
}

func max(x, y int) int {
    if x > y {
        return x
    }
    return y
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

## Approach 2: Optimized Dynamic Programming with Two Auxiliary Arrays

### Intuition:
Optimize the brute force DP approach by precomputing the maximum values that can be carried from each side (left and right) for each row, thereby reducing the time complexity.

1. Use two auxiliary arrays `left` and `right` to store the maximum scores obtainable from the left and from the right for each row.
2. The `left` array considers the maximum score from left to right for row `i-1`, and similarly, the `right` array considers from right to left.

### Time Complexity:
- O(m * n) where m is the number of rows and n is the number of columns.

### Space Complexity:
- O(n) due to using additional arrays `left` and `right`.

```go
func maxPoints(points [][]int) int {
    m, n := len(points), len(points[0])
    
    prev := make([]int, n)
    copy(prev, points[0])
    
    for i := 1; i < m; i++ {
        left := make([]int, n)
        right := make([]int, n)
        
        // Fill `left` array
        left[0] = prev[0]
        for j := 1; j < n; j++ {
            left[j] = max(left[j - 1] - 1, prev[j])
        }
        
        // Fill `right` array
        right[n - 1] = prev[n - 1]
        for j := n - 2; j >= 0; j-- {
            right[j] = max(right[j + 1] - 1, prev[j])
        }
        
        // Update current values in `prev` for the next iteration using `left` and `right`
        for j := 0; j < n; j++ {
            prev[j] = points[i][j] + max(left[j], right[j])
        }
    }
    
    // Get maximum from the last computed `prev`
    result := 0
    for _, score := range prev {
        result = max(result, score)
    }
    return result
}
```

## Approach 3: Optimized Dynamic Programming with Prefix and Suffix Arrays

### Intuition:
Similar to using separate `left` and `right` arrays but combine the changes in one pass using prefix and suffix maximum computations. This further simplifies understanding and potentially optimizes cache performance.

### Time Complexity:
- O(m * n)

### Space Complexity:
- O(n)

```go
func maxPoints(points [][]int) int {
    m, n := len(points), len(points[0])
    
    dp := make([]int, n)
    copy(dp, points[0])
    
    for i := 1; i < m; i++ {
        prefixMax := make([]int, n)
        suffixMax := make([]int, n)
        
        prefixMax[0] = dp[0]
        for j := 1; j < n; j++ {
            prefixMax[j] = max(prefixMax[j - 1] - 1, dp[j])
        }
        
        suffixMax[n - 1] = dp[n - 1]
        for j := n - 2; j >= 0; j-- {
            suffixMax[j] = max(suffixMax[j + 1] - 1, dp[j])
        }
        
        for j := 0; j < n; j++ {
            dp[j] = points[i][j] + max(prefixMax[j], suffixMax[j])
        }
    }
    
    result := 0
    for _, score := range dp {
        result = max(result, score)
    }
    return result
}
```

With these approaches, you can handle the problem more efficiently and appreciate the incremental optimization steps.

