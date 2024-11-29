# 115. [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Approach 1: Recursion with Memoization

### Solution
go
```go
// Time Complexity: O(n * m)
// Space Complexity: O(n * m)
func numDistinct(s string, t string) int {
    // Memoization table
    memo := make([][]int, len(s))
    for i := range memo {
        memo[i] = make([]int, len(t))
        for j := range memo[i] {
            memo[i][j] = -1
        }
    }

    return numDistinctHelper(s, t, 0, 0, memo)
}

func numDistinctHelper(s string, t string, sIndex, tIndex int, memo [][]int) int {
    // If t is exhausted, one subsequence is found
    if tIndex == len(t) {
        return 1
    }
    // If s is exhausted first, no subsequence can be formed
    if sIndex == len(s) {
        return 0
    }
    // Check memoization table
    if memo[sIndex][tIndex] != -1 {
        return memo[sIndex][tIndex]
    }

    // If characters match, both decisions can be made
    if s[sIndex] == t[tIndex] {
        memo[sIndex][tIndex] = numDistinctHelper(s, t, sIndex+1, tIndex+1, memo) +
                               numDistinctHelper(s, t, sIndex+1, tIndex, memo)
    } else {
        // If characters don't match, skip the current character in s
        memo[sIndex][tIndex] = numDistinctHelper(s, t, sIndex+1, tIndex, memo)
    }

    return memo[sIndex][tIndex]
}
```

## Approach 2: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n * m)
// Space Complexity: O(n * m)
func numDistinct(s string, t string) int {
    // DP table
    dp := make([][]int, len(s)+1)
    for i := range dp {
        dp[i] = make([]int, len(t)+1)
    }

    // Base case: empty t can be formed by all possible substrings of s
    for i := 0; i <= len(s); i++ {
        dp[i][len(t)] = 1
    }

    // Fill the table in reverse order
    for i := len(s) - 1; i >= 0; i-- {
        for j := len(t) - 1; j >= 0; j-- {
            if s[i] == t[j] {
                // Both using the character and ignoring it
                dp[i][j] = dp[i+1][j+1] + dp[i+1][j]
            } else {
                // Ignore the character in s
                dp[i][j] = dp[i+1][j]
            }
        }
    }

    // Result is the number of ways to form t[0...m] from s[0...n]
    return dp[0][0]
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
go
```go
// Time Complexity: O(n * m)
// Space Complexity: O(m)
func numDistinct(s string, t string) int {
    // Previous and current row for space optimization
    prev := make([]int, len(t)+1)
    curr := make([]int, len(t)+1)

    // Base case: empty t can be formed by all possible substrings of s
    for i := 0; i <= len(s); i++ {
        prev[len(t)] = 1
    }

    // Fill the table in reverse order
    for i := len(s) - 1; i >= 0; i-- {
        for j := len(t) - 1; j >= 0; j-- {
            if s[i] == t[j] {
                curr[j] = prev[j+1] + prev[j]
            } else {
                curr[j] = prev[j]
            }
        }
        // Move current row to previous for next iteration
        copy(prev, curr)
    }

    // Result is the number of ways to form t[0...m] from s[0...n]
    return prev[0]
}
```

