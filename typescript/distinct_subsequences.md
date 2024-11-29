# 115. [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Approach 1: Recursion with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(n * m)
// Space Complexity: O(n * m)
class Solution {
    numDistinct(s: string, t: string): number {
        // Memoization table
        const memo: number[][] = Array.from({ length: s.length }, () => Array(t.length).fill(-1));
        
        return this.numDistinctHelper(s, t, 0, 0, memo);
    }

    private numDistinctHelper(s: string, t: string, sIndex: number, tIndex: number, memo: number[][]): number {
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
        
        // If characters match, both decisions can be made
        if (s[sIndex] === t[tIndex]) {
            memo[sIndex][tIndex] = this.numDistinctHelper(s, t, sIndex + 1, tIndex + 1, memo) +
                                   this.numDistinctHelper(s, t, sIndex + 1, tIndex, memo);
        } else {
            // If characters don't match, skip the current character in s
            memo[sIndex][tIndex] = this.numDistinctHelper(s, t, sIndex + 1, tIndex, memo);
        }

        return memo[sIndex][tIndex];
    }
}
```

## Approach 2: Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(n * m)
// Space Complexity: O(n * m)
class Solution {
    numDistinct(s: string, t: string): number {
        // DP table
        const dp: number[][] = Array.from({ length: s.length + 1 }, () => Array(t.length + 1).fill(0));
        
        // Base case: empty t can be formed by all possible substrings of s
        for (let i = 0; i <= s.length; i++) {
            dp[i][t.length] = 1;
        }
        
        // Fill the table in reverse order
        for (let i = s.length - 1; i >= 0; i--) {
            for (let j = t.length - 1; j >= 0; j--) {
                if (s[i] === t[j]) {
                    // Both using the character and ignoring it
                    dp[i][j] = dp[i + 1][j + 1] + dp[i + 1][j];
                } else {
                    // Ignore the character in s
                    dp[i][j] = dp[i + 1][j];
                }
            }
        }
        
        // Result is the number of ways to form t[0...m] from s[0...n]
        return dp[0][0];
    }
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
typescript
```typescript
// Time Complexity: O(n * m)
// Space Complexity: O(m)
class Solution {
    numDistinct(s: string, t: string): number {
        // Previous and current row for space optimization
        let prev: number[] = new Array(t.length + 1).fill(0);
        let curr: number[] = new Array(t.length + 1).fill(0);
        
        // Base case: empty t can be formed by all possible substrings of s
        prev[t.length] = 1;

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
            prev = [...curr];
        }
        
        // Result is the number of ways to form t[0...m] from s[0...n]
        return prev[0];
    }
}
```

