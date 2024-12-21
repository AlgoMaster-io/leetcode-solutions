# [LeetCode 688: Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

In this problem, you are given a chessboard of size N x N, a starting position (r, c), and a number of moves `K`. The task is to determine the probability that the knight remains on the board after moving exactly K times.

## Approaches:

1. [Recursive Brute Force with Memoization](#recursive-brute-force-with-memoization)
2. [Dynamic Programming](#dynamic-programming)

---

### Approach 1: Recursive Brute Force with Memoization

Intuition:
- The problem can be solved using basic recursion by simulating each possible move from the knight's current position.
- At each step, check if the knight is within bounds. If it moves off the board, the probability of that path is zero.
- Since the same state (position and remaining moves) can be reached in multiple ways, use memoization to store the results of overlapping subproblems to avoid re-computation.

Steps:
1. Define a recursive function to simulate knight moves, stopping when out of bounds or moves are exhausted.
2. For each valid move, recursively calculate the probability of staying on the board.
3. Use a dictionary or 2D array as a memoization table to store results of each state.

Time Complexity: O(N^2 * K), where N is the board size and K is the number of moves.

Space Complexity: O(N^2 * K) for the memoization table.

```csharp
public class Solution {
    private int[] dx = new int[] {1, 2, 2, 1, -1, -2, -2, -1};
    private int[] dy = new int[] {2, 1, -1, -2, -2, -1, 1, 2};
    private double[,,] memo;
    
    public double KnightProbability(int N, int K, int r, int c) {
        memo = new double[N, N, K + 1];
        
        return DFS(N, K, r, c);
    }
    
    private double DFS(int N, int K, int r, int c) {
        // If out of bounds
        if (r < 0 || r >= N || c < 0 || c >= N) return 0;
        // If no moves left, still on board
        if (K == 0) return 1;
        // If already computed
        if (memo[r, c, K] != 0) return memo[r, c, K];
        
        double probability = 0;
        for (int i = 0; i < 8; i++) {
            // Recursively calculate for all possible moves
            probability += 0.125 * DFS(N, K - 1, r + dx[i], c + dy[i]);
        }
        
        memo[r, c, K] = probability;
        return probability;
    }
}
```

---

### Approach 2: Dynamic Programming

Intuition:
- Translate the recursive solution into iterative DP to efficiently compute the probability of staying on the board.
- Use a DP table `dp[i][r][c]` to represent the probability of the knight being at position (r, c) after `i` moves.
- Initialize `dp[0][r][c] = 1` as the base case where no move has been made.
- Accumulate the probabilities for each move to update `dp[i][r][c]`.

Steps:
1. Initialize a 3D DP array with dimensions for moves, rows, and columns.
2. Start from the base case and iterate for each move up to K.
3. During each iteration, update the DP table based on feasible knight moves.

Time Complexity: O(N^2 * K) due to iterating over each position for each move.

Space Complexity: O(N^2 * K) for the DP table, but it can be optimized further.

```csharp
public class Solution {
    public double KnightProbability(int N, int K, int r, int c) {
        double[,,] dp = new double[K + 1, N, N];
        dp[0, r, c] = 1;
        
        int[] dx = new int[] {1, 2, 2, 1, -1, -2, -2, -1};
        int[] dy = new int[] {2, 1, -1, -2, -2, -1, 1, 2};
        
        for (int step = 1; step <= K; step++) {
            for (int x = 0; x < N; x++) {
                for (int y = 0; y < N; y++) {
                    for (int move = 0; move < 8; move++) {
                        int nx = x + dx[move];
                        int ny = y + dy[move];
                        if (nx >= 0 && nx < N && ny >= 0 && ny < N) {
                            dp[step, nx, ny] += dp[step - 1, x, y] / 8.0;
                        }
                    }
                }
            }
        }
        
        double result = 0;
        for (int x = 0; x < N; x++) {
            for (int y = 0; y < N; y++) {
                result += dp[K, x, y];
            }
        }
        
        return result;
    }
}
```

Both of these approaches efficiently compute the probability that the knight remains on the board after K moves with robust explanations and meticulous implementation details.

