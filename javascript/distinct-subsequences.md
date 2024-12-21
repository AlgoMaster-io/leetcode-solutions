# [Leetcode 115: Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)

---

### Recursive Approach

Intuition:  
The idea is to use recursion to explore all possible subsequences. For each character in the source string, we have two choices: either to include it in the subsequence if it matches the current character in the target string or to skip it. The base cases are when the target is exhausted, which means one valid subsequence is found, or when the source is exhausted before the target, which means no valid subsequence can be formed. 

```javascript
function numDistinct(s, t) {
    function recurse(i, j) {
        // If we have matched all characters in t
        if (j === t.length) return 1;
        
        // If we have exhausted the string s
        if (i === s.length) return 0;
        
        // Choices: skip s[i] or use it if it matches t[j]
        let count = 0;
        if (s[i] === t[j]) {
            count += recurse(i + 1, j + 1); // Use s[i]
        }
        count += recurse(i + 1, j); // Skip s[i]
        
        return count;
    }
    
    return recurse(0, 0);
}
```

**Time Complexity:** O(2^m), where m is the length of `s`.  
**Space Complexity:** O(n), where n is the recursion stack depth.

---

### Memoization Approach

Intuition:  
To optimize the recursive approach, we use a memoization table to store the results of subproblems. This avoids redundant calculations and reduces the exponential complexity. Each state in our recursive function is determined by indices i and j, therefore we use a 2D memo array to cache the results.

```javascript
function numDistinct(s, t) {
    const memo = new Map();
    
    function recurse(i, j) {
        // Base cases
        if (j === t.length) return 1;
        if (i === s.length) return 0;
        
        const key = `${i},${j}`;
        if (memo.has(key)) return memo.get(key);
        
        let count = 0;
        if (s[i] === t[j]) {
            count += recurse(i + 1, j + 1); // Use s[i]
        }
        count += recurse(i + 1, j); // Skip s[i]

        memo.set(key, count);
        return count;
    }
    
    return recurse(0, 0);
}
```

**Time Complexity:** O(m * n), where m is the length of `s` and n is the length of `t`.  
**Space Complexity:** O(m * n) for the memo array.

---

### Dynamic Programming Approach

Intuition:  
We use a 2D DP table where `dp[i][j]` represents the number of distinct subsequences of `t`[0..j-1] in `s`[0..i-1]. The key observation is:
- If `s[i-1]` equals `t[j-1]`, then `dp[i][j] = dp[i-1][j-1] + dp[i-1][j]`.
- If they aren't equal, `dp[i][j] = dp[i-1][j]`.  
The reason is we have the option to either match these equal characters or continue without matching them.

```javascript
function numDistinct(s, t) {
    const m = s.length, n = t.length;
    if (n > m) return 0;
    
    const dp = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    
    // Base case: an empty target string
    for (let i = 0; i <= m; i++) {
        dp[i][0] = 1;
    }
    
    // Fill the DP table
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (s[i - 1] === t[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    
    return dp[m][n];
}
```

**Time Complexity:** O(m * n)  
**Space Complexity:** O(m * n)

This DP approach is the most efficient as it systematically computes the count of subsequences by iterating over all possible character pairs, keeping the computation manageable.

