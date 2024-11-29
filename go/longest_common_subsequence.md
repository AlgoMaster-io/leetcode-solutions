# 1143. [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

## Approach 1: Recursive Approach

### Solution
go
```go
// Time Complexity: Exponential
// Space Complexity: O(m + n) for the recursion stack
package main

import "fmt"

func longestCommonSubsequence(text1 string, text2 string) int {
    return lcs(text1, text2, len(text1), len(text2))
}

func lcs(s1 string, s2 string, m int, n int) int {
    // Base case: If either string is empty, return 0
    if m == 0 || n == 0 {
        return 0
    }
    
    // If the last characters match, reduce both lengths and add 1 to the result
    if s1[m-1] == s2[n-1] {
        return 1 + lcs(s1, s2, m-1, n-1)
    } else {
        // Otherwise, take the maximum result by reducing one string at a time
        return max(lcs(s1, s2, m-1, n), lcs(s1, s2, m, n-1))
    }
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
package main

func longestCommonSubsequence(text1 string, text2 string) int {
    m, n := len(text1), len(text2)
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }

    // Fill the DP table iteratively
    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if text1[i-1] == text2[j-1] {
                // Matching characters, take diagonal value and add 1
                dp[i][j] = 1 + dp[i-1][j-1]
            } else {
                // Different characters, take the maximum of left or top cell
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
            }
        }
    }

    // The bottom-right cell contains the length of LCS
    return dp[m][n]
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
go
```go
// Time Complexity: O(m * n)
// Space Complexity: O(min(m, n))
package main

func longestCommonSubsequence(text1 string, text2 string) int {
    // Ensure text1 is larger or equal
    if len(text1) < len(text2) {
        return longestCommonSubsequence(text2, text1)
    }
    
    prev := make([]int, len(text2)+1)

    // Iterate through each character of text1
    for i := 1; i <= len(text1); i++ {
        curr := make([]int, len(text2)+1)

        // Iterate through each character of text2
        for j := 1; j <= len(text2); j++ {
            if text1[i-1] == text2[j-1] {
                curr[j] = 1 + prev[j-1]
            } else {
                curr[j] = max(curr[j-1], prev[j])
            }
        }
        prev = curr // Move current row to previous for the next iteration
    }

    return prev[len(text2)] // Last computed row contains the result
}
```


