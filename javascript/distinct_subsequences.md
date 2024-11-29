# 115. [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Approach 1: Recursion with Memoization

### Solution
```javascript
// Time Complexity: O(n * m)
// Space Complexity: O(n * m)
var numDistinct = function(s, t) {
    // Memoization table
    const memo = Array.from({ length: s.length }, () => Array(t.length).fill(-1));
    
    function numDistinctHelper(s, t, sIndex, tIndex, memo) {
        // If t is exhausted, one subsequence is found
        if (tIndex === t.length) {
            return 1;
        }
        // If s is exhausted first, no subsequence can be formed
        if (sIndex === s.length) {
            return 0;
        }
        // Check memoization table
        if (memo[sIndex][tIndex] !== -1) {
            return memo[sIndex][tIndex];
        }
        
        // Calculate based on character match
        if (s[sIndex] === t[tIndex]) {
            memo[sIndex][tIndex] = numDistinctHelper(s, t, sIndex + 1, tIndex + 1, memo) + 
                                   numDistinctHelper(s, t, sIndex + 1, tIndex, memo);
        } else {
            memo[sIndex][tIndex] = numDistinctHelper(s, t, sIndex + 1, tIndex, memo);
        }
        
        return memo[sIndex][tIndex];
    }
    
    return numDistinctHelper(s, t, 0, 0, memo);
};
```

## Approach 2: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n * m)
// Space Complexity: O(n * m)
var numDistinct = function(s, t) {
    // DP table
    const dp = Array.from({ length: s.length + 1 }, () => Array(t.length + 1).fill(0));

    // Base case: empty t can be formed by all possible substrings of s
    for (let i = 0; i <= s.length; i++) {
        dp[i][t.length] = 1;
    }

    // Fill the table in reverse order
    for (let i = s.length - 1; i >= 0; i--) {
        for (let j = t.length - 1; j >= 0; j--) {
            if (s[i] === t[j]) {
                dp[i][j] = dp[i + 1][j + 1] + dp[i + 1][j];
            } else {
                dp[i][j] = dp[i + 1][j];
            }
        }
    }
    
    // Result is the number of ways to form t[0...m] from s[0...n]
    return dp[0][0];
};
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
```javascript
// Time Complexity: O(n * m)
// Space Complexity: O(m)
var numDistinct = function(s, t) {
    // Previous and current row for space optimization
    let prev = new Array(t.length + 1).fill(1);
    let curr = new Array(t.length + 1).fill(0);
    
    // Fill the table in reverse order
    for (let i = s.length - 1; i >= 0; i--) {
        for (let j = t.length - 1; j >= 0; j--) {
            if (s[i] === t[j]) {
                curr[j] = prev[j + 1] + prev[j];
            } else {
                curr[j] = prev[j];
            }
        }
        // Move current row to previous for next iteration
        prev = curr.slice(); // Use slice to create a new copy of the current array
    }
    
    // Result is the number of ways to form t[0...m] from s[0...n]
    return prev[0];
};
```

