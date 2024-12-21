# [LeetCode 1143: Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization (Top-Down DP)](#memoization-top-down-dp)
3. [Tabulation (Bottom-Up DP)](#tabulation-bottom-up-dp)

---

## Recursive Approach

### Intuition
The longest common subsequence problem is a classic problem that can often be solved using recursion. The idea is simple: compare the last characters of the two strings. If they match, the solution is `1 + LCS(rest of string1, rest of string2)`. If they don't match, we find the LCS excluding the last character from either string and return the maximum of the two scenarios. This naturally leads to a recursive definition.

### Code

```typescript
function longestCommonSubsequenceRecursive(text1: string, text2: string): number {
    function lcsHelper(m: number, n: number): number {
        // Base case: if either string is empty, no common subsequence
        if (m === 0 || n === 0) return 0;
        
        // If the last characters match, find LCS for the strings excluding the last char
        if (text1[m - 1] === text2[n - 1]) {
            return 1 + lcsHelper(m - 1, n - 1);
        } else {
            // If they don't match, consider both possibilities
            return Math.max(lcsHelper(m - 1, n), lcsHelper(m, n - 1));
        }
    }
    
    return lcsHelper(text1.length, text2.length);
}
```

### Time and Space Complexity
- **Time Complexity**: O(2^(m+n)), where m is the length of `text1` and n is the length of `text2`. This is because in the worst case, the recursive approach checks all combinations.
- **Space Complexity**: O(m+n) due to recursion stack space.

---

## Memoization (Top-Down DP)

### Intuition
The recursive solution solves many subproblems repeatedly. To optimize, we can store the results of subproblems in a memoization table and reuse them whenever needed. This way, we can avoid the exponential time complexity of the recursive solution.

### Code

```typescript
function longestCommonSubsequenceMemo(text1: string, text2: string): number {
    const memo: number[][] = Array(text1.length).fill(null).map(() => Array(text2.length).fill(-1));
    
    function lcsHelper(m: number, n: number): number {
        // Base case
        if (m === 0 || n === 0) return 0;
        
        // Check if result is already computed
        if (memo[m-1][n-1] !== -1) return memo[m-1][n-1];
        
        // Compute result and store in memo
        if (text1[m - 1] === text2[n - 1]) {
            memo[m-1][n-1] = 1 + lcsHelper(m - 1, n - 1);
        } else {
            memo[m-1][n-1] = Math.max(lcsHelper(m - 1, n), lcsHelper(m, n - 1));
        }
        
        return memo[m-1][n-1];
    }
    
    return lcsHelper(text1.length, text2.length);
}
```

### Time and Space Complexity
- **Time Complexity**: O(m × n) because each subproblem is evaluated only once.
- **Space Complexity**: O(m × n) for the memoization table.

---

## Tabulation (Bottom-Up DP)

### Intuition
A tabulation approach iteratively fills a table that represents subproblem solutions. By iteratively building the table from the ground up, we can get the solution by looking at the final entry in the table. This method avoids recursion and often results in more efficient code.

### Code

```typescript
function longestCommonSubsequenceTabulation(text1: string, text2: string): number {
    const m = text1.length;
    const n = text2.length;
    const dp: number[][] = Array(m + 1).fill(0).map(() => Array(n + 1).fill(0));
    
    // Fill the dp table iteratively
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            // If characters match, take diagonal + 1
            if (text1[i - 1] === text2[j - 1]) {
                dp[i][j] = 1 + dp[i - 1][j - 1];
            } else {
                // Take the maximum value from either excluding current char in text1 or text2
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    
    // The value in dp[m][n] is the result
    return dp[m][n];
}
```

### Time and Space Complexity
- **Time Complexity**: O(m × n), as we fill in a table of size m x n where each entry depends on constant-time operations.
- **Space Complexity**: O(m × n) for the dp table. However, this can be optimized to O(min(m, n)) if we only keep the current and previous row (or column) in memory at any moment.

