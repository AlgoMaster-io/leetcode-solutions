## [LeetCode 1143: Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

### Approaches
- [Recursive Approach](#recursive-approach)
- [Memoization Approach](#memoization-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)

### Recursive Approach

#### Intuition:
The longest common subsequence problem can be approached using simple recursion. The basic idea is to compare the characters of the two strings from the end. If they match, then they are part of the LCS, and we move both pointers. If they don't match, we have the option to either move the pointer of the first string or the second string, and take the maximum of these two scenarios.

#### Code:
```go
func longestCommonSubsequenceRecursive(text1 string, text2 string) int {
    return recursiveLCS(len(text1)-1, len(text2)-1, text1, text2)
}

func recursiveLCS(i, j int, text1, text2 string) int {
    // Base case: if any string is exhausted, return 0
    if i < 0 || j < 0 {
        return 0
    }
    
    // If characters match, add 1 and move both pointers
    if text1[i] == text2[j] {
        return 1 + recursiveLCS(i-1, j-1, text1, text2)
    }
    
    // If not matching, find the max by moving each pointer separately
    return max(recursiveLCS(i-1, j, text1, text2), recursiveLCS(i, j-1, text1, text2))
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

#### Time Complexity:
- The time complexity is O(2^(m+n)), where m is the length of `text1` and n is the length of `text2`, due to the overlap of subproblems.

#### Space Complexity:
- The space complexity is O(m+n) for the recursion stack.

### Memoization Approach

#### Intuition:
To overcome the exponential time complexity of the recursive approach, we use a memoization technique. We store the results of previously solved subproblems in a 2D array, which avoids recalculating the same values.

#### Code:
```go
func longestCommonSubsequenceMemo(text1 string, text2 string) int {
    m, n := len(text1), len(text2)
    memo := make([][]int, m)
    for i := range memo {
        memo[i] = make([]int, n)
        for j := range memo[i] {
            memo[i][j] = -1
        }
    }
    return memoLCS(m-1, n-1, text1, text2, memo)
}

func memoLCS(i, j int, text1, text2 string, memo [][]int) int {
    if i < 0 || j < 0 {
        return 0
    }
    
    if memo[i][j] != -1 {
        return memo[i][j]
    }
    
    if text1[i] == text2[j] {
        memo[i][j] = 1 + memoLCS(i-1, j-1, text1, text2, memo)
    } else {
        memo[i][j] = max(memoLCS(i-1, j, text1, text2, memo), memoLCS(i, j-1, text1, text2, memo))
    }
    
    return memo[i][j]
}
```

#### Time Complexity:
- The time complexity is O(m * n), as each state is computed once and cache is utilized.

#### Space Complexity:
- The space complexity is O(m * n) for the memo array.

### Dynamic Programming Approach

#### Intuition:
We use a 2D DP array where `dp[i][j]` represents the length of LCS for substrings `text1[0...i]` and `text2[0...j]`. The main idea is to build the solution to larger problems by using solutions to smaller subproblems.

- If the characters `text1[i-1]` and `text2[j-1]` are equal, then `dp[i][j] = 1 + dp[i-1][j-1]`.
- If they are not equal, then `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.

#### Code:
```go
func longestCommonSubsequenceDP(text1 string, text2 string) int {
    m, n := len(text1), len(text2)
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }
    
    // Fill the dp array from 1 to m and 1 to n
    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if text1[i-1] == text2[j-1] {
                dp[i][j] = 1 + dp[i-1][j-1]
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
            }
        }
    }
    
    return dp[m][n]
}
```

#### Time Complexity:
- The time complexity is O(m * n), due to filling the DP table.

#### Space Complexity:
- The space complexity is O(m * n) due to the DP table. However, with further optimization, it's possible to reduce this space complexity to O(min(m, n)).

