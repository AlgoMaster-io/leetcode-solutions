# 688. [Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Approach 1: Recursion with Memoization

### Solution
```javascript
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2 * k)
const knightProbability = (N, K, r, c) => {
    const DIRECTIONS = [
        [1, 2], [1, -2], [-1, 2], [-1, -2],
        [2, 1], [2, -1], [-2, 1], [-2, -1]
    ];
    const memo = Array.from({ length: N }, () => 
                Array.from({ length: N }, () => Array(K + 1).fill(null)));
    
    const findProbability = (N, K, r, c, memo) => {
        if (r < 0 || r >= N || c < 0 || c >= N) return 0;
        if (K === 0) return 1;
        if (memo[r][c][K] !== null) return memo[r][c][K];

        let prob = 0;
        for (let direction of DIRECTIONS) {
            const newRow = r + direction[0];
            const newCol = c + direction[1];
            prob += findProbability(N, K - 1, newRow, newCol, memo);
        }

        memo[r][c][K] = prob;
        return prob;
    };

    return findProbability(N, K, r, c, memo) / Math.pow(8, K);
};
```

## Approach 2: Dynamic Programming (Bottom-Up)

### Solution
```javascript
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2)
const knightProbability = (N, K, r, c) => {
    const DIRECTIONS = [
        [1, 2], [1, -2], [-1, 2], [-1, -2],
        [2, 1], [2, -1], [-2, 1], [-2, -1]
    ];
    let dp0 = Array.from({ length: N }, () => Array(N).fill(0));
    dp0[r][c] = 1;

    for (let step = 0; step < K; step++) {
        let dp1 = Array.from({ length: N }, () => Array(N).fill(0));
        for (let i = 0; i < N; i++) {
            for (let j = 0; j < N; j++) {
                if (dp0[i][j] > 0) {
                    for (let direction of DIRECTIONS) {
                        const ni = i + direction[0];
                        const nj = j + direction[1];
                        if (ni >= 0 && ni < N && nj >= 0 && nj < N) {
                            dp1[ni][nj] += dp0[i][j] / 8.0;
                        }
                    }
                }
            }
        }
        dp0 = dp1;
    }

    let result = 0;
    for (let row of dp0) {
        for (let x of row) {
            result += x;
        }
    }
    return result;
};
```

## Approach 3: Space-Optimized Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2)
const knightProbability = (N, K, r, c) => {
    const DIRECTIONS = [
        [1, 2], [1, -2], [-1, 2], [-1, -2],
        [2, 1], [2, -1], [-2, 1], [-2, -1]
    ];
    let current = Array.from({ length: N }, () => Array(N).fill(0));
    current[r][c] = 1;

    for (let step = 0; step < K; step++) {
        let next = Array.from({ length: N }, () => Array(N).fill(0));
        for (let i = 0; i < N; i++) {
            for (let j = 0; j < N; j++) {
                if (current[i][j] > 0) {
                    for (let direction of DIRECTIONS) {
                        const ni = i + direction[0];
                        const nj = j + direction[1];
                        if (ni >= 0 && ni < N && nj >= 0 && nj < N) {
                            next[ni][nj] += current[i][j] / 8.0;
                        }
                    }
                }
            }
        }
        current = next;
    }

    let result = 0;
    for (let row of current) {
        for (let x of row) {
            result += x;
        }
    }
    return result;
};
```

