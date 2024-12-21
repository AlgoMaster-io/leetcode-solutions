# [Leetcode 688: Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Approaches
- [Approach 1: Recursive DFS with Memoization](#approach-1-recursive-dfs-with-memoization)
- [Approach 2: Dynamic Programming (Bottom-Up)](#approach-2-dynamic-programming-bottom-up)

### Approach 1: Recursive DFS with Memoization

**Intuition:**
We can simulate the knight's movements by using a recursive DFS approach. Starting from the given position, we explore all possible moves the knight can make, and keep track of the number of valid paths that stay within the boundaries of the chessboard.

To optimize our recursive DFS, we use memoization to store the results of subproblems that have already been computed. This avoids recomputation and speeds up the process significantly.

**Steps:**
1. Define a recursive function `dfs(r, c, k)` that returns the probability of the knight staying on the board after `k` moves from position `(r, c)`.
2. For each move, check if the knight stays within bounds. If it falls off the board, return a probability of 0.
3. Use memoization to store results of previously computed `(r, c, k)` states.
4. For each possible move, update the probability and return the final calculated probability.

**Code:**

```go
func knightProbability(N int, K int, r int, c int) float64 {
    // Possible moves of the knight
    moves := [][]int{{-2, -1}, {-1, -2}, {1, -2}, {2, -1}, {2, 1}, {1, 2}, {-1, 2}, {-2, 1}}
    
    // Memoization map
    memo := make(map[string]float64)
    
    // Helper function for memoization
    var dfs func(r, c, k int) float64
    dfs = func(r, c, k int) float64 {
        // Check if out of bounds
        if r < 0 || r >= N || c < 0 || c >= N {
            return 0
        }
        // If no moves left
        if k == 0 {
            return 1
        }
        
        // Generate a key for memoization purposes
        key := fmt.Sprintf("%d,%d,%d", r, c, k)
        if val, exists := memo[key]; exists {
            return val
        }
        
        // Initialize probability for this state as 0
        prob := 0.0
        // Explore all potential 8 moves
        for _, move := range moves {
            prob += dfs(r + move[0], c + move[1], k - 1) / 8.0
        }
        
        // Memoize the result
        memo[key] = prob
        
        return prob
    }
    
    // Start DFS from the initial position
    return dfs(r, c, K)
}
```

**Time Complexity:** O(N^2 * K), since we are potentially computing the probability for each cell `(r, c)` for each `k` time.

**Space Complexity:** O(N^2 * K), for the memoization table.

### Approach 2: Dynamic Programming (Bottom-Up)

**Intuition:**
The recursive approach can be efficiently implemented using a bottom-up dynamic programming approach. We iteratively build the solution by calculating the probability of staying on the board for `k` moves from any given position.

We maintain a dp table where `dp[k][r][c]` represents the probability of the knight being on the board at position `(r, c)` after `k` moves. We update the table by considering all possible moves of the knight similar to the recursive approach.

**Steps:**
1. Initialize a 3D array `dp` with dimensions `[K+1][N][N]` for memoization, starting with all zeroes.
2. Base case: `dp[0][r][c] = 1` (probability 1 when no moves).
3. For each level `k` from `1 to K`, calculate the probability for each `(r, c)` by considering previous positions from `k-1`.
4. For each position, accumulate the probability of reaching it using the knight's legal moves.
5. The desired answer is `dp[K][r][c]` which sums the probabilities of the knight staying on board after `K` moves starting from `(r, c)`.

**Code:**

```go
func knightProbability(N int, K int, r int, c int) float64 {
    // Possible moves of the knight
    moves := [][]int{{-2, -1}, {-1, -2}, {1, -2}, {2, -1}, {2, 1}, {1, 2}, {-1, 2}, {-2, 1}}
    
    // Initialize dp array
    dp := make([][][]float64, K+1)
    for k := range dp {
        dp[k] = make([][]float64, N)
        for i := range dp[k] {
            dp[k][i] = make([]float64, N)
        }
    }
    // Base case: Start at r,c
    dp[0][r][c] = 1.0
    
    // Fill dp table
    for k := 1; k <= K; k++ {
        for i := 0; i < N; i++ {
            for j := 0; j < N; j++ {
                for _, move := range moves {
                    ni, nj := i + move[0], j + move[1]
                    if ni >= 0 && ni < N && nj >= 0 && nj < N {
                        dp[k][i][j] += dp[k-1][ni][nj] / 8.0
                    }
                }
            }
        }
    }
    
    // Sum probabilities of ending at any position
    prob := 0.0
    for i := 0; i < N; i++ {
        for j := 0; j < N; j++ {
            prob += dp[K][i][j]
        }
    }
    
    return prob
}
```

**Time Complexity:** O(N^2 * K), since we process every cell `(r, c)` for each `k` level.

**Space Complexity:** O(N^2 * K), storing probabilities for all cells and moves in the dp table.

