# 72. [Edit Distance](https://leetcode.com/problems/edit-distance/)

## Approach 1: Recursive Solution

### Solution
typescript
```typescript
// Time Complexity: O(3^(m+n))
// Space Complexity: O(m+n)
class Solution {
    public minDistance(word1: string, word2: string): number {
        return this.computeDistance(word1, word2, word1.length, word2.length);
    }

    private computeDistance(word1: string, word2: string, m: number, n: number): number {
        if (m === 0) return n;
        if (n === 0) return m;

        if (word1[m - 1] === word2[n - 1]) {
            return this.computeDistance(word1, word2, m - 1, n - 1);
        }

        return 1 + Math.min(
            this.computeDistance(word1, word2, m, n - 1), // Insert
            Math.min(
                this.computeDistance(word1, word2, m - 1, n), // Remove
                this.computeDistance(word1, word2, m - 1, n - 1) // Replace
            )
        );
    }
}
```

## Approach 2: Dynamic Programming (2D Table)

### Solution
typescript
```typescript
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
class Solution {
    public minDistance(word1: string, word2: string): number {
        const m = word1.length;
        const n = word2.length;
        const dp: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));

        for (let i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        for (let j = 0; j <= n; j++) {
            dp[0][j] = j;
        }

        for (let i = 1; i <= m; i++) {
            for (let j = 1; j <= n; j++) {
                if (word1[i - 1] === word2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1];
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
typescript
```typescript
// Time Complexity: O(m * n)
// Space Complexity: O(n)
class Solution {
    public minDistance(word1: string, word2: string): number {
        const m = word1.length;
        const n = word2.length;
        let prev: number[] = Array(n + 1).fill(0);
        let curr: number[] = Array(n + 1).fill(0);

        for (let j = 0; j <= n; j++) {
            prev[j] = j;
        }

        for (let i = 1; i <= m; i++) {
            curr[0] = i;
            for (let j = 1; j <= n; j++) {
                if (word1[i - 1] === word2[j - 1]) {
                    curr[j] = prev[j - 1];
                } else {
                    curr[j] = 1 + Math.min(
                        prev[j], // Remove
                        Math.min(
                            curr[j - 1], // Insert
                            prev[j - 1] // Replace
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

