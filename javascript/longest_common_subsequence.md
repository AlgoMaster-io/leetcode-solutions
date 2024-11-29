# 1143. [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

## Approach 1: Recursive Approach

### Solution
```javascript
// Time Complexity: Exponential
// Space Complexity: O(m + n) for the recursion stack
class Solution {
    longestCommonSubsequence(text1, text2) {
        return this.lcs(text1, text2, text1.length, text2.length);
    }

    lcs(s1, s2, m, n) {
        // Base case: If either string is empty, return 0
        if (m === 0 || n === 0) {
            return 0;
        }

        // If the last characters match, reduce both lengths and add 1 to the result
        if (s1[m - 1] === s2[n - 1]) {
            return 1 + this.lcs(s1, s2, m - 1, n - 1);
        } else {
            // Otherwise, take the maximum result by reducing one string at a time
            return Math.max(this.lcs(s1, s2, m - 1, n), this.lcs(s1, s2, m, n - 1));
        }
    }
}
```

## Approach 2: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
class Solution {
    longestCommonSubsequence(text1, text2) {
        const m = text1.length;
        const n = text2.length;
        const dp = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));

        // Fill the DP table iteratively
        for (let i = 1; i <= m; i++) {
            for (let j = 1; j <= n; j++) {
                if (text1[i - 1] === text2[j - 1]) {
                    // Matching characters, take diagonal value and add 1
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    // Different characters, take the maximum of left or top cell
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        // The bottom-right cell contains the length of LCS
        return dp[m][n];
    }
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
```javascript
// Time Complexity: O(m * n)
// Space Complexity: O(min(m, n))
class Solution {
    longestCommonSubsequence(text1, text2) {
        if (text1.length < text2.length) {
            return this.longestCommonSubsequence(text2, text1); // Ensure text1 is larger or equal
        }

        let prev = new Array(text2.length + 1).fill(0);

        // Iterate through each character of text1
        for (let i = 1; i <= text1.length; i++) {
            let curr = new Array(text2.length + 1).fill(0);

            // Iterate through each character of text2
            for (let j = 1; j <= text2.length; j++) {
                if (text1[i - 1] === text2[j - 1]) {
                    curr[j] = 1 + prev[j - 1];
                } else {
                    curr[j] = Math.max(curr[j - 1], prev[j]);
                }
            }
            prev = curr; // Move current row to previous for the next iteration
        }

        return prev[text2.length]; // Last computed row contains the result
    }
}
```

