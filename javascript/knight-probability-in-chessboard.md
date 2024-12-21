# [Leetcode 688: Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Approaches
- [Approach 1: Recursive Depth-First Search (DFS)](#approach-1-recursive-depth-first-search-dfs)
- [Approach 2: Dynamic Programming (DP) Top-Down with Memoization](#approach-2-dynamic-programming-dp-top-down-with-memoization)
- [Approach 3: Dynamic Programming (DP) Bottom-Up](#approach-3-dynamic-programming-dp-bottom-up)

### Approach 1: Recursive Depth-First Search (DFS)

The first and most intuitive solution is to use recursion to simulate all possible moves of the knight and calculate the probability of remaining on the board after `k` moves.

#### Intuition
- Start the knight at position `(r, c)` on an `N x N` board, and try all 8 possible moves for the knight.
- For each move, check if the knight moves within the board boundaries.
- If the knight moves out of bounds, that path contributes a probability of `0`.
- If the knight is within bounds, recursively calculate the probability for the remaining `k-1` moves.
- Accumulate the probabilities from the valid knight moves, dividing by `8` since each move has an equal probability of `1/8`.

#### Time Complexity
- The time complexity is `O(8^k)` as there are `8` possible moves for each of the `k` steps.
  
#### Space Complexity
- The space complexity is `O(k)` due to recursion stack depth.

#### Code

```javascript
const knightProbability = function(N, K, r, c) {
    const directions = [
        [2, 1], [1, 2], [-1, 2], [-2, 1], 
        [-2, -1], [-1, -2], [1, -2], [2, -1]
    ];

    const dfs = (i, j, k) => {
        // If the knight is out of bounds, probability is 0
        if (i < 0 || i >= N || j < 0 || j >= N) return 0;

        // If no more moves, the knight is still on the board
        if (k === 0) return 1;

        let prob = 0;
        for (const [dx, dy] of directions) {
            prob += dfs(i + dx, j + dy, k - 1) / 8.0;
        }
        return prob;
    };
  
    return dfs(r, c, K);
};
```

### Approach 2: Dynamic Programming (DP) Top-Down with Memoization

To improve on the inefficiency of the recursive solution, we can use memoization to store already computed probabilities for certain states `(i, j, k)`.

#### Intuition
- Use a memoization table `dp(i, j, k)` to store the probability of remaining on board with `k` moves from position `(i, j)`.
- If we already computed a probability for a state, reuse it instead of recalculating.

#### Time Complexity
- The time complexity reduces to `O(N^2 * k)` since each state `(i, j, k)` is computed once.

#### Space Complexity
- The space complexity becomes `O(N^2 * k)` for storing memoized states.

#### Code

```javascript
const knightProbability = function(N, K, r, c) {
    const directions = [
        [2, 1], [1, 2], [-1, 2], [-2, 1], 
        [-2, -1], [-1, -2], [1, -2], [2, -1]
    ];

    const memo = Array.from({ length: N }, () => 
        Array.from({ length: N }, () =>
            Array(K + 1).fill(null)
        )
    );

    const dfs = (i, j, k) => {
        // If out of bounds
        if (i < 0 || i >= N || j < 0 || j >= N) return 0;
        // No moves left
        if (k === 0) return 1;
        // Check memoization
        if (memo[i][j][k] !== null) return memo[i][j][k];

        let prob = 0;
        for (const [dx, dy] of directions) {
            prob += dfs(i + dx, j + dy, k - 1) / 8.0;
        }
        memo[i][j][k] = prob;
        return prob;
    };
  
    return dfs(r, c, K);
};
```

### Approach 3: Dynamic Programming (DP) Bottom-Up

Instead of recursively computing the states, we can build the DP solution iteratively.

#### Intuition
- Use a 3D table `dp` where `dp[i][j][k]` represents the probability of staying on the board starting from `(i, j)` with `k` moves.
- Initialize `dp[i][j][0] = 1` for all `(i,j)` because with 0 moves, the knight does not move.
- Iteratively calculate probabilities for increasing values of `k` from 1 to `K`.

#### Time Complexity
- The time complexity is `O(N^2 * k)`.

#### Space Complexity
- The space complexity is `O(N^2)` as only the current and next state grids are needed.

#### Code

```javascript
const knightProbability = function(N, K, r, c) {
    const directions = [
        [2, 1], [1, 2], [-1, 2], [-2, 1], 
        [-2, -1], [-1, -2], [1, -2], [2, -1]
    ];

    let dp1 = Array.from({ length: N }, () => Array(N).fill(0));
    dp1[r][c] = 1;

    for (let k = 0; k < K; k++) {
        const dp2 = Array.from({ length: N }, () => Array(N).fill(0));
        for (let i = 0; i < N; i++) {
            for (let j = 0; j < N; j++) {
                if (dp1[i][j] > 0) {
                    for (const [dx, dy] of directions) {
                        const ni = i + dx;
                        const nj = j + dy;
                        if (ni >= 0 && ni < N && nj >= 0 && nj < N) {
                            dp2[ni][nj] += dp1[i][j] / 8;
                        }
                    }
                }
            }
        }
        dp1 = dp2;
    }

    return dp1.flat().reduce((acc, val) => acc + val, 0);
};
```

This code iteratively builds the probabilities for `k` moves and utilizes space efficiently by storing only the previous and current states probabilities.

