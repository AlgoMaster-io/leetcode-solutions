# 72. [Edit Distance](https://leetcode.com/problems/edit-distance/)

## Approach 1: Recursive Solution

### Solution
```javascript
// Time Complexity: O(3^(m+n))
// Space Complexity: O(m+n)
class Solution {
    minDistance(word1, word2) {
        // Helper method to compute edit distance recursively
        return this.computeDistance(word1, word2, word1.length, word2.length);
    }

    computeDistance(word1, word2, m, n) {
        // If first string is empty, return length of second string
        if (m === 0) return n;

        // If second string is empty, return length of first string
        if (n === 0) return m;

        // If characters are the same, continue with next characters
        if (word1.charAt(m - 1) === word2.charAt(n - 1)) {
            return this.computeDistance(word1, word2, m - 1, n - 1);
        }

        // Calculate minimum operation from insert, remove, or replace
        return 1 + Math.min(
            this.computeDistance(word1, word2, m, n - 1), // Insert
            Math.min(
                this.computeDistance(word1, word2, m - 1, n),    // Remove
                this.computeDistance(word1, word2, m - 1, n - 1) // Replace
            )
        );
    }
}
```

## Approach 2: Dynamic Programming (2D Table)

### Solution
```javascript
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
class Solution {
    minDistance(word1, word2) {
        const m = word1.length;
        const n = word2.length;

        // Create DP table
        const dp = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));

        // Initialize base cases
        for (let i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        for (let j = 0; j <= n; j++) {
            dp[0][j] = j;
        }

        // Fill in DP table
        for (let i = 1; i <= m; i++) {
            for (let j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) === word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1]; // Characters match, no new operation
                } else {
                    dp[i][j] = 1 + Math.min(
                        dp[i - 1][j], // Remove
                        Math.min(
                            dp[i][j - 1], // Insert
                            dp[i - 1][j - 1] // Replace
                        )
                    );
                }
            }
        }

        return dp[m][n];
    }
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
```javascript
// Time Complexity: O(m * n)
// Space Complexity: O(n)
class Solution {
    minDistance(word1, word2) {
        const m = word1.length;
        const n = word2.length;

        // Create two arrays for dynamic programming, only need two rows at a time
        let prev = Array(n + 1).fill(0);
        let curr = Array(n + 1).fill(0);

        // Initialize base case for the first row
        for (let j = 0; j <= n; j++) {
            prev[j] = j;
        }

        // Fill in DP table
        for (let i = 1; i <= m; i++) {
            curr[0] = i; // Base case for the first column
            for (let j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) === word2.charAt(j - 1)) {
                    curr[j] = prev[j - 1]; // Characters match
                } else {
                    curr[j] = 1 + Math.min(
                        prev[j], // Remove
                        Math.min(
                            curr[j - 1], // Insert
                            prev[j - 1]  // Replace
                        )
                    );
                }
            }
            [prev, curr] = [curr, prev];
        }

        return prev[n];
    }
}
```

