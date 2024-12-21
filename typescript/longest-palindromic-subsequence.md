# [Leetcode 516: Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Approaches:
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Memoization (Top-Down Dynamic Programming)](#approach-2-memoization-top-down-dynamic-programming)
- [Approach 3: Bottom-Up Dynamic Programming](#approach-3-bottom-up-dynamic-programming)

---

## Approach 1: Recursive Solution

### Intuition:
The problem can be broken down by examining subproblems where we check if the first and last characters match. If they do, they contribute to the palindrome. Otherwise, we consider the problem in two parts: excluding the first character and excluding the last character.

### Code:
```typescript
function longestPalindromeSubseq(s: string): number {
    const helper = (start: number, end: number): number => {
        // Base case: Single character or empty string.
        if (start > end) return 0;
        if (start === end) return 1;
        
        // If the characters at both ends are the same.
        if (s[start] === s[end]) {
            return 2 + helper(start + 1, end - 1);
        } else {
            // If they are not the same, proceed with both possibilities.
            return Math.max(helper(start + 1, end), helper(start, end - 1));
        }
    }
    
    return helper(0, s.length - 1);
}
```

### Time complexity:
- Exponential: O(2^n), where n is the length of the string due to the overlap of subproblems.

### Space complexity:
- O(n) due to the recursive call stack.

---

## Approach 2: Memoization (Top-Down Dynamic Programming)

### Intuition:
To optimize the recursive approach, store the result of each subproblem in a 2D array so we do not recompute results for overlapping subproblems.

### Code:
```typescript
function longestPalindromeSubseq(s: string): number {
    const n = s.length;
    const memo: number[][] = Array.from({length: n}, () => Array(n).fill(undefined));
    
    const dp = (start: number, end: number): number => {
        if (start > end) return 0;
        if (start === end) return 1;
        if (memo[start][end] !== undefined) return memo[start][end];
        
        if (s[start] === s[end]) {
            memo[start][end] = 2 + dp(start + 1, end - 1);
        } else {
            memo[start][end] = Math.max(dp(start + 1, end), dp(start, end - 1));
        }
        
        return memo[start][end];
    }
    
    return dp(0, n - 1);
}
```

### Time complexity:
- O(n^2) since each subproblem is solved once and stored.

### Space complexity:
- O(n^2) for the memoization table.

---

## Approach 3: Bottom-Up Dynamic Programming

### Intuition:
Build up a table from smallest subproblems to larger ones. The idea is to fill a 2D table where dp[i][j] holds the length of the longest palindromic subsequence in the substring from index i to j.

### Code:
```typescript
function longestPalindromeSubseq(s: string): number {
    const n = s.length;
    const dp: number[][] = Array.from({length: n}, () => Array(n).fill(0));
    
    // Single letters are palindrome of length 1
    for (let i = 0; i < n; i++) {
        dp[i][i] = 1;
    }
    
    // Fill the table
    for (let len = 2; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const j = i + len - 1;
            if (s[i] === s[j]) {
                dp[i][j] = 2 + dp[i + 1][j - 1];
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    
    return dp[0][n - 1];
}
```

### Time complexity:
- O(n^2) due to nested iteration over the length of the string.

### Space complexity:
- O(n^2) for the 2D dynamic programming table.

