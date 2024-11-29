# 688. [Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Approach 1: Recursion with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(N^2 * K)
// Space Complexity: O(N^2 * K)
const DIRECTIONS: number[][] = [
    [1, 2], [1, -2], [-1, 2], [-1, -2],
    [2, 1], [2, -1], [-2, 1], [-2, -1]
];

function knightProbability(N: number, K: number, r: number, c: number): number {
    const memo: (number | null)[][][] = Array.from({ length: N }, () => Array.from({ length: N }, () => Array(K + 1).fill(null)));
    return findProbability(N, K, r, c, memo) / Math.pow(8, K);
}

function findProbability(N: number, K: number, r: number, c: number, memo: (number | null)[][][]): number {
    if (r < 0 || r >= N || c < 0 || c >= N) return 0;
    if (K === 0) return 1;
    if (memo[r][c][K] !== null) return memo[r][c][K]!;

    let prob = 0;
    for (const direction of DIRECTIONS) {
        const newRow = r + direction[0];
        const newCol = c + direction[1];
        prob += findProbability(N, K - 1, newRow, newCol, memo);
    }

    memo[r][c][K] = prob;
    return prob;
}
```

## Approach 2: Dynamic Programming (Bottom-Up)

### Solution
typescript
```typescript
// Time Complexity: O(N^2 * K)
// Space Complexity: O(N^2)
const DIRECTIONS: number[][] = [
    [1, 2], [1, -2], [-1, 2], [-1, -2],
    [2, 1], [2, -1], [-2, 1], [-2, -1]
];

function knightProbability(N: number, K: number, r: number, c: number): number {
    let dp0: number[][] = Array.from({ length: N }, () => Array(N).fill(0));
    dp0[r][c] = 1;

    for (let step = 0; step < K; step++) {
        const dp1: number[][] = Array.from({ length: N }, () => Array(N).fill(0));
        for (let i = 0; i < N; i++) {
            for (let j = 0; j < N; j++) {
                if (dp0[i][j] > 0) {
                    for (const direction of DIRECTIONS) {
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
    for (const row of dp0) {
        for (const x of row) {
            result += x;
        }
    }
    return result;
}
```

## Approach 3: Space-Optimized Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(N^2 * K)
// Space Complexity: O(N^2)
const DIRECTIONS: number[][] = [
    [1, 2], [1, -2], [-1, 2], [-1, -2],
    [2, 1], [2, -1], [-2, 1], [-2, -1]
];

function knightProbability(N: number, K: number, r: number, c: number): number {
    let current: number[][] = Array.from({ length: N }, () => Array(N).fill(0));
    current[r][c] = 1;

    for (let step = 0; step < K; step++) {
        const next: number[][] = Array.from({ length: N }, () => Array(N).fill(0));
        for (let i = 0; i < N; i++) {
            for (let j = 0; j < N; j++) {
                if (current[i][j] > 0) {
                    for (const direction of DIRECTIONS) {
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
    for (const row of current) {
        for (const x of row) {
            result += x;
        }
    }
    return result;
}
```

