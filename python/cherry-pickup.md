# [LeetCode 741: Cherry Pickup](https://leetcode.com/problems/cherry-pickup/)

## Approaches
1. [Recursive DFS with Memoization](#approach-1-recursive-dfs-with-memoization)
2. [Dynamic Programming - 3D DP table](#approach-2-dynamic-programming-3d-dp-table)

## Approach 1: Recursive DFS with Memoization

**Intuition:**
The problem can be broken down into two separate traversals, where the goal is to maximize the number of cherries collected in a round trip from the start to the end and back to the start. This method uses recursion and memoization to efficiently explore valid paths and store intermediate results.

**Steps:**
- Define a recursive function `cherryPickup(r1, c1, r2)` where `(r1, c1)` is the position of one traversal and `(r2, c2)` the mirrored position of the other traversal, since if `(r1 + c1) == (r2 + c2)` then they represent two states of the same single traversal.
- Use memoization to store results for overlapping subproblems.
- Base cases include checking for boundaries and thorns represented by `-1`.
- If both traversals reach `(n-1, n-1)`, return the cherries at the position (if any).
- Use the results of recursion to calculate the maximum cherries collected by choosing one of the four possible moves `(r1 + 1, c1)`, `(r1, c1 + 1)` and `(r2 + 1, c2)`, `(r2, c2 + 1)`.

**Time Complexity:** `O(N^3)`
**Space Complexity:** `O(N^3)`

```python
class Solution:
    def cherryPickup(self, grid: List[List[int]]) -> int:
        N = len(grid)
        memo = {}
        
        def dp(r1, c1, r2):
            # Calculate corresponding c2 because r1 + c1 == r2 + c2
            c2 = r1 + c1 - r2
            
            # Base cases
            if r1 == N or c1 == N or r2 == N or c2 == N or \
               grid[r1][c1] == -1 or grid[r2][c2] == -1:
                return -float('inf')
            elif r1 == N-1 and c1 == N-1:
                return grid[r1][c1]
            
            # Check memoization
            if (r1, c1, r2) in memo:
                return memo[(r1, c1, r2)]
            
            # Recursion with possible moves
            result = grid[r1][c1]
            if r1 != r2:
                result += grid[r2][c2]
            
            result += max(dp(r1+1, c1, r2+1),   # Both go down
                          dp(r1, c1+1, r2),     # First goes right, second goes down
                          dp(r1+1, c1, r2),     # First goes down, second goes right
                          dp(r1, c1+1, r2+1))   # Both go right
            
            memo[(r1, c1, r2)] = result
            return result
        
        return max(0, dp(0, 0, 0))  # Max with 0 to handle the case with no valid path
```

## Approach 2: Dynamic Programming - 3D DP table

**Intuition:**
Using a 3D DP array is a natural continuation from the recursive solution where we precalculate and fill up a DP table instead of relying on function calls. This avoids the overhead of recursion and offers a more systematic computation of the cherry collection path.

**Steps:**
- Use a 3D DP table `dp[r1][c1][r2]` where each entry corresponds to the maximum cherries collected when the first traversal is at `(r1, c1)` and the second at `(r2, c2)`, calculated by `c2 = r1 + c1 - r2`.
- Initialize `dp[N-1][N-1][N-1]` with the cherries present at the bottom-right corner.
- Fill the DP table from the bottom up, considering valid paths only.
  
**Time Complexity:** `O(N^3)`
**Space Complexity:** `O(N^3)`

```python
class Solution:
    def cherryPickup(self, grid: List[List[int]]) -> int:
        N = len(grid)
        dp = [[[-1] * N for _ in range(N)] for _ in range(N)]
        
        dp[N-1][N-1][N-1] = grid[N-1][N-1]
        
        for r1 in range(N-1, -1, -1):
            for c1 in range(N-1, -1, -1):
                for r2 in range(N-1, -1, -1):
                    c2 = r1 + c1 - r2
                    if c2 < 0 or c2 >= N or grid[r1][c1] == -1 or grid[r2][c2] == -1:
                        continue
                    
                    cherries = grid[r1][c1]
                    if r1 != r2:
                        cherries += grid[r2][c2]

                    best_next = -1
                    if r1 + 1 < N and r2 + 1 < N:
                        best_next = max(best_next, dp[r1+1][c1][r2+1])  # Both down
                    if r1 + 1 < N and c2 + 1 < N:
                        best_next = max(best_next, dp[r1+1][c1][r2])  # First down, second right
                    if c1 + 1 < N and r2 + 1 < N:
                        best_next = max(best_next, dp[r1][c1+1][r2+1])  # First right, second down
                    if c1 + 1 < N and c2 + 1 < N:
                        best_next = max(best_next, dp[r1][c1+1][r2])  # Both right

                    if best_next != -1:
                        dp[r1][c1][r2] = cherries + best_next
                    elif r1 == N-1 and c1 == N-1:
                        # Base case catch if coming from bottom-most reachable corner
                        dp[r1][c1][r2] = cherries
                    else:
                        dp[r1][c1][r2] = -float('inf')  # -inf otherwise

        return max(0, dp[0][0][0])  # Max with 0 to handle the case with no valid path
```

Both approaches provide efficient ways to calculate the maximum cherries that can be collected in a grid, handling various edge cases by ensuring that only valid paths are considered.

