# 808. [Soup Servings](https://leetcode.com/problems/soup-servings/)

## Approach 1: Recursive with Memoization

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)

class Solution {
    constructor() {
        this.memo = new Map();
    }

    soupServings(N) {
        if (N >= 5000) return 1.0; // Threshold optimization
        return this.serve(N, N);
    }

    serve(a, b) {
        if (a <= 0 && b <= 0) return 0.5; // Both soups are finished
        if (a <= 0) return 1; // Only soup A finishes
        if (b <= 0) return 0; // Only soup B finishes
        
        const key = `${a},${b}`;
        if (this.memo.has(key)) return this.memo.get(key);

        const probability = 0.25 * (
            this.serve(a - 100, b) +
            this.serve(a - 75, b - 25) +
            this.serve(a - 50, b - 50) +
            this.serve(a - 25, b - 75)
        );

        this.memo.set(key, probability);
        return probability;
    }
}
```

## Approach 2: Iterative with Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)

class Solution {
    soupServings(N) {
        if (N >= 5000) return 1.0; // Threshold optimization
        N = Math.ceil(N / 25); // Reduce and round up

        const dp = Array.from({ length: N + 1 }, () => Array(N + 1).fill(0));
        dp[0][0] = 0.5; // Base case: both soups are empty
        for (let i = 1; i <= N; i++) {
            dp[i][0] = 0; // Soup B empty first
            dp[0][i] = 1; // Soup A empty first
        }

        for (let i = 1; i <= N; i++) {
            for (let j = 1; j <= N; j++) {
                dp[i][j] = 0.25 * (
                    this.getProbability(dp, i - 4, j) + 
                    this.getProbability(dp, i - 3, j - 1) + 
                    this.getProbability(dp, i - 2, j - 2) + 
                    this.getProbability(dp, i - 1, j - 3)
                );
            }
        }

        return dp[N][N];
    }

    getProbability(dp, a, b) {
        if (a <= 0 && b <= 0) return 0.5; // Both finish at the same time
        if (a <= 0) return 1; // Only soup A finishes
        if (b <= 0) return 0; // Only soup B finishes
        return dp[a][b];
    }
}
```

