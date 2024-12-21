# [Leetcode 97: Interleaving String](https://leetcode.com/problems/interleaving-string/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization (Top-Down Dynamic Programming)](#memoization-top-down-dynamic-programming)
3. [Bottom-Up Dynamic Programming](#bottom-up-dynamic-programming)

### Recursive Approach

In this naive recursive approach, we explore all possible ways to form the interleaved string `s3` by picking characters from `s1` and `s2`. 

#### Intuition:
- Consider pointers `i` for `s1`, `j` for `s2`, and `k` for `s3`.
- Check if `s1[i]` matches `s3[k]`. If it does, recursively check for `s1[i+1:]` and `s3[k+1:]`.
- Similarly, check if `s2[j]` matches `s3[k]`. If it does, recursively check for `s2[j+1:]` and `s3[k+1:]`.
- The recursion ends if `k` reaches the end of `s3`, in which case we have successfully matched both `s1` and `s2` to form `s3`.

```go
func isInterleaveRecursive(s1 string, s2 string, s3 string) bool {
    if len(s1) + len(s2) != len(s3) {
        return false
    }
    return recursiveHelper(s1, s2, s3, 0, 0, 0)
}

func recursiveHelper(s1, s2, s3 string, i, j, k int) bool {
    if k == len(s3) {
        return true
    }
    
    matchS1, matchS2 := false, false
    
    if i < len(s1) && s1[i] == s3[k] {
        matchS1 = recursiveHelper(s1, s2, s3, i+1, j, k+1)
    }
    
    if j < len(s2) && s2[j] == s3[k] {
        matchS2 = recursiveHelper(s1, s2, s3, i, j+1, k+1)
    }
    
    return matchS1 || matchS2
}
```

#### Complexity:
- Time complexity: O(2^(m+n)), where m and n are the lengths of `s1` and `s2`, respectively.
- Space complexity: O(m+n) (maximum depth of recursion).

### Memoization (Top-Down Dynamic Programming)

To optimize the recursive approach, we use a memoization technique to store intermediate results and avoid repeated computations.

#### Intuition:
- We use a 2D array `memo` to store results of subproblems defined by indices `(i, j)` for substrings of `s1` and `s2`.
- If we have already computed a result for indices `(i, j)`, we use it directly instead of recomputing.

```go
func isInterleaveMemo(s1 string, s2 string, s3 string) bool {
    if len(s1) + len(s2) != len(s3) {
        return false
    }
    memo := make([][]int, len(s1)+1)
    for i := range memo {
        memo[i] = make([]int, len(s2)+1)
        for j := range memo[i] {
            memo[i][j] = -1
        }
    }
    return memoHelper(s1, s2, s3, 0, 0, memo)
}

func memoHelper(s1, s2, s3 string, i, j int, memo [][]int) bool {
    if i+j == len(s3) {
        return true
    }
    
    if memo[i][j] != -1 {
        return memo[i][j] == 1
    }
    
    matchS1, matchS2 := false, false
    
    if i < len(s1) && s1[i] == s3[i+j] {
        matchS1 = memoHelper(s1, s2, s3, i+1, j, memo)
    }
    
    if j < len(s2) && s2[j] == s3[i+j] {
        matchS2 = memoHelper(s1, s2, s3, i, j+1, memo)
    }
    
    memo[i][j] = 0
    if matchS1 || matchS2 {
        memo[i][j] = 1
    }
    
    return memo[i][j] == 1
}
```

#### Complexity:
- Time complexity: O(m*n), where m and n are the lengths of `s1` and `s2`, respectively.
- Space complexity: O(m*n), for the memoization table.

### Bottom-Up Dynamic Programming

The bottom-up dynamic programming approach constructs the solution iteratively using a table to keep track of results for subproblems.

#### Intuition:
- We create a 2D DP array `dp` where `dp[i][j]` is true if `s3[0:i+j]` is an interleaving of `s1[0:i]` and `s2[0:j]`.
- Fill the table iteratively based on the conditions explained above.

```go
func isInterleaveDP(s1 string, s2 string, s3 string) bool {
    if len(s1) + len(s2) != len(s3) {
        return false
    }
    
    dp := make([][]bool, len(s1) + 1)
    for i := range dp {
        dp[i] = make([]bool, len(s2) + 1)
    }
    
    dp[0][0] = true // An empty s1 and s2 can interleave to form an empty s3

    for i := 0; i <= len(s1); i++ {
        for j := 0; j <= len(s2); j++ {
            if i > 0 && s1[i-1] == s3[i+j-1] {
                dp[i][j] = dp[i][j] || dp[i-1][j]  // Can we form s3 by extending s1?
            }
            if j > 0 && s2[j-1] == s3[i+j-1] {
                dp[i][j] = dp[i][j] || dp[i][j-1]  // Can we form s3 by extending s2?
            }
        }
    }
    return dp[len(s1)][len(s2)]
}
```

#### Complexity:
- Time complexity: O(m*n), where m and n are the lengths of `s1` and `s2`, respectively.
- Space complexity: O(m*n), for the DP table. 

This approach gives the optimal solution as it minimizes repeated calculations and efficiently uses space compared to the recursive implementations.

