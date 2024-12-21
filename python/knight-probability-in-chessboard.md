# [688. Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Solutions

- [Brute Force Recursion](#brute-force-recursion)
- [Memoization](#memoization-dp-top-down)
- [Dynamic Programming - Bottom Up](#dynamic-programming-bottom-up)

---

### Brute Force Recursion

#### Intuition

The problem can be approached by simulating the knight's possible moves recursively. Given a start position (r, c) on an `N x N` chessboard, we can explore each of the knight's 8 possible moves. For each valid move, recursively calculate the probability of staying on the board for `K-1` remaining moves. The base cases occur when K=0 and the knight is on the board or when it moves out of bounds.

#### Approach

1. Define the 8 potential moves a knight can make.
2. Recursively attempt each move decreasing `K` until K=0.
3. If the knight is within bounds when `K=0`, it counts as part of the probability.
4. Aggregate these probabilities and divide by 8 (total possible moves) at every recursive call.

```python
def knightProbability(N, K, r, c):
    def dfs(N, K, r, c):
        # If knight is out of bounds
        if r < 0 or r >= N or c < 0 or c >= N:
            return 0
        # If no more moves allowed, valid position
        if K == 0:
            return 1
        prob = 0
        # List of all 8 possible moves of a knight
        knight_moves = [(2, 1), (2, -1), (-2, 1), (-2, -1), 
                        (1, 2), (1, -2), (-1, 2), (-1, -2)]
        for dr, dc in knight_moves:
            prob += dfs(N, K - 1, r + dr, c + dc) / 8
        return prob
    
    return dfs(N, K, r, c)
```

- **Time Complexity**: O(8^K), because in the worst case every move tries all 8 options.
- **Space Complexity**: O(K), due to the call stack from recursion.

---

### Memoization (DP Top-Down)

#### Intuition

The recursive approach has overlapping subproblems due to different paths leading to the same board position with the same number of moves remaining. To optimize, store results of subproblems in a memoization table to avoid redundant calculations.

#### Approach

1. Utilize a memoization table `dp[K][r][c]` to store the probability of reaching `(r, c)` with `K` moves remaining.
2. Check if the current state has already been computed to return its value directly.
3. Otherwise, compute it using recursion as before.

```python
def knightProbability(N, K, r, c):
    memo = [[[-1 for _ in range(N)] for _ in range(N)] for _ in range(K+1)]

    def dfs(K, r, c):
        if r < 0 or r >= N or c < 0 or c >= N:
            return 0
        if K == 0:
            return 1
        if memo[K][r][c] != -1:
            return memo[K][r][c]

        prob = 0
        knight_moves = [(2, 1), (2, -1), (-2, 1), (-2, -1), 
                        (1, 2), (1, -2), (-1, 2), (-1, -2)]
        for dr, dc in knight_moves:
            prob += dfs(K - 1, r + dr, c + dc) / 8

        memo[K][r][c] = prob
        return prob

    return dfs(K, r, c)
```

- **Time Complexity**: O(N^2 * K), as each state `(K, r, c)` is computed once.
- **Space Complexity**: O(N^2 * K), for the memoization table.

---

### Dynamic Programming (Bottom Up)

#### Intuition

Transform the top-down memoization approach into a bottom-up dynamic programming approach. We iteratively calculate probabilities starting from zero moves upward to `K` moves. This avoids recursion overhead and reduces code complexity.

#### Approach

1. Initialize a DP table `dp[][]` with K layers representing moves left. 
2. Start with base cases: for `K=0`, the probability is 1 at `(r, c)` if on board.
3. For each `k` (1 to K), update the probability at `(r, c)` based on probabilities at `k-1` from positions reachable via knight's move.

```python
def knightProbability(N, K, r, c):
    # dp[k][i][j] means probability of stay at position i, j after k moves
    dp = [[[0 for _ in range(N)] for _ in range(N)] for _ in range(K+1)]
    dp[0][r][c] = 1
    
    knight_moves = [(2, 1), (2, -1), (-2, 1), (-2, -1), 
                    (1, 2), (1, -2), (-1, 2), (-1, -2)]
    
    for k in range(1, K+1):
        for i in range(N):
            for j in range(N):
                for dr, dc in knight_moves:
                    ni, nj = i + dr, j + dc
                    if 0 <= ni < N and 0 <= nj < N:
                        dp[k][i][j] += dp[k-1][ni][nj] / 8
    
    # Result is total probability sum after K moves for all the board
    total_probability = sum(dp[K][i][j] for i in range(N) for j in range(N))
    return total_probability
```

- **Time Complexity**: O(N^2 * K)
- **Space Complexity**: O(N^2), reduced to one current and one previous DP matrix if in-place update.

