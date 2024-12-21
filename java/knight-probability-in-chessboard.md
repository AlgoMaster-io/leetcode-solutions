# [688. Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Approaches
- [Brute Force Recursive Approach](#brute-force-recursive-approach)
- [Memoization Approach](#memoization-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)

### Brute Force Recursive Approach
In this approach, we use recursion to explore all possible moves of the knight and calculate the probability of staying within the boundaries of the chessboard. The knight has 8 possible moves from any position, and we simulate every possible sequence of moves from the starting position for `k` steps.

#### Intuition:
- Start at the initial position and make all possible moves recursively.
- For each move, check if the new position is still inside the board.
- If it's inside, recursively explore the next move.
- Base case: If no more moves (`k` becomes zero), we've successfully completed a sequence.

```java
public class Solution {
    private static final int[][] DIRECTIONS = {
        {2, 1}, {2, -1}, {-2, 1}, {-2, -1},
        {1, 2}, {1, -2}, {-1, 2}, {-1, -2}
    };

    public double knightProbability(int N, int K, int row, int column) {
        return knightProbabilityRec(N, K, row, column) / Math.pow(8, K);
    }
    
    private int knightProbabilityRec(int N, int K, int row, int column) {
        if (row < 0 || row >= N || column < 0 || column >= N) return 0;
        if (K == 0) return 1;

        int totalWays = 0;
        for (int[] direction : DIRECTIONS) {
            totalWays += knightProbabilityRec(N, K - 1, row + direction[0], column + direction[1]);
        }
        return totalWays;
    }
}
```

**Time Complexity**: O(8^K) - There are 8 possible moves and we perform a recursive call for each move, K times.
**Space Complexity**: O(K) - Recursive call stack space.

### Memoization Approach
We use memoization to optimize the recursive approach by storing the already computed probabilities for certain positions with given steps. This prevents recalculating probabilities for the same state, significantly reducing the number of computations.

#### Intuition:
- Use a 3D array to save the probability results for position `(i, j)` with `k` moves remaining.
- If the probability has been calculated for a specific state, reuse it instead of recalculating.

```java
public class Solution {
    private static final int[][] DIRECTIONS = {
        {2, 1}, {2, -1}, {-2, 1}, {-2, -1},
        {1, 2}, {1, -2}, {-1, 2}, {-1, -2}
    };

    public double knightProbability(int N, int K, int row, int column) {
        double[][][] memo = new double[N][N][K + 1];
        return dp(N, K, row, column, memo);
    }
    
    private double dp(int N, int K, int row, int column, double[][][] memo) {
        if (row < 0 || row >= N || column < 0 || column >= N) return 0;
        if (K == 0) return 1;
        if (memo[row][column][K] != 0) return memo[row][column][K];

        double probability = 0;
        for (int[] direction : DIRECTIONS) {
            probability += dp(N, K - 1, row + direction[0], column + direction[1], memo) / 8;
        }
        memo[row][column][K] = probability;
        return probability;
    }
}
```

**Time Complexity**: O(N^2 * K) - Each state `(i, j, k)` is computed once.
**Space Complexity**: O(N^2 * K) - 3D memoization array.

### Dynamic Programming Approach
This approach uses a bottom-up dynamic programming table where `dp[i][j][k]` represents the probability of the knight being at position `(i, j)` with `k` moves remaining. We build the solution from the ground up.

#### Intuition:
- Initialize the base case (when k=0).
- Each position at step `k` is derived from the probabilities at step `k-1`.
- Sum up all probabilities when k=0.

```java
public class Solution {
    public double knightProbability(int N, int K, int row, int column) {
        double[][][] dp = new double[K+1][N][N];
        dp[0][row][column] = 1;

        int[][] moves = {
            {2, 1}, {2, -1}, {-2, 1}, {-2, -1},
            {1, 2}, {1, -2}, {-1, 2}, {-1, -2}
        };

        for (int k = 1; k <= K; k++) {
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    for (int[] move : moves) {
                        int ni = i + move[0];
                        int nj = j + move[1];
                        if (ni >= 0 && ni < N && nj >= 0 && nj < N) {
                            dp[k][i][j] += dp[k-1][ni][nj] / 8.0;
                        }
                    }
                }
            }
        }

        double result = 0.0;
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                result += dp[K][i][j];
            }
        }
        return result;
    }
}
```

**Time Complexity**: O(N^2 * K) - Iterating over N^2 cells for each step K.
**Space Complexity**: O(N^2 * K) - 3D DP table.

Each method provides a different balance between complexity and performance, with the Dynamic Programming approach being the most efficient for larger input sizes.

