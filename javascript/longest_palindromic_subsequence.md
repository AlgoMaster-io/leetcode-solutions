# 516. [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Approach 1: Recursive Solution with Memoization

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
class Solution {
    longestPalindromeSubseq(s) {
        const memo = Array.from({ length: s.length }, () => Array(s.length).fill(0));
        return this.longestPalindromeSubseqHelper(s, 0, s.length - 1, memo);
    }

    longestPalindromeSubseqHelper(s, start, end, memo) {
        // Base case: single character is a palindrome
        if (start === end) {
            return 1;
        }
        // Base case: no characters (invalid scenario)
        if (start > end) {
            return 0;
        }
        // Check memo
        if (memo[start][end] !== 0) {
            return memo[start][end];
        }
        // If characters match, continue inward
        if (s[start] === s[end]) {
            memo[start][end] = 2 + this.longestPalindromeSubseqHelper(s, start + 1, end - 1, memo);
        } else {
            // Explore both options and take the max
            memo[start][end] = Math.max(
                this.longestPalindromeSubseqHelper(s, start + 1, end, memo),
                this.longestPalindromeSubseqHelper(s, start, end - 1, memo)
            );
        }
        return memo[start][end];
    }
}
```

## Approach 2: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
class Solution {
    longestPalindromeSubseq(s) {
        const n = s.length;
        const dp = Array.from({ length: n }, () => Array(n).fill(0));

        // Every single character is a palindrome of length 1
        for (let i = 0; i < n; i++) {
            dp[i][i] = 1;
        }

        // Check subproblems of increasing lengths
        for (let length = 2; length <= n; length++) {
            for (let start = 0; start <= n - length; start++) {
                const end = start + length - 1;
                // Characters match, add 2 + result for inner substring
                if (s[start] === s[end]) {
                    dp[start][end] = dp[start + 1][end - 1] + 2;
                } else {
                    // Choose the best option from excluding either end
                    dp[start][end] = Math.max(dp[start + 1][end], dp[start][end - 1]);
                }
            }
        }

        // Result is in dp[0][n-1]
        return dp[0][n - 1];
    }
}
```

## Approach 3: Optimized Dynamic Programming with Reduced Space

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
class Solution {
    longestPalindromeSubseq(s) {
        const n = s.length;
        let dp = new Array(n).fill(0);
        let prevDp = new Array(n).fill(0);

        // Every single character is a palindrome of length 1
        for (let i = 0; i < n; i++) {
            dp[i] = 1;
        }

        // Check subproblems of increasing lengths
        for (let length = 2; length <= n; length++) {
            for (let start = 0; start <= n - length; start++) {
                const end = start + length - 1;
                const temp = dp[start]; // Preserve current value before overwriting

                if (s[start] === s[end]) {
                    dp[start] = prevDp[start + 1] + 2;
                } else {
                    dp[start] = Math.max(dp[start + 1], prevDp[start]);
                }

                prevDp[start] = temp; // Update prevDp for the next iteration
            }
        }

        // Result is in dp[0]
        return dp[0];
    }
}
```

