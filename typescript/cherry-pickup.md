# [Cherry Pickup](https://leetcode.com/problems/cherry-pickup/)

## Approaches
1. [Approach 1: Naive Recursive Solution](#approach-1)
2. [Approach 2: Dynamic Programming - 3D DP](#approach-2)
3. [Approach 3: Dynamic Programming - Optimized 2D DP](#approach-3)

## Problem Description
You are given a `N x N` grid representing a field of cherries. Each cell in the grid has one of three values:

* `0` meaning the cell is empty,
* `1` meaning the cell contains a cherry,
* `-1` meaning the cell contains a thorn and cannot be traversed.

Your task is to return the maximum number of cherries that can be collected by a trip from `(0, 0)` to `(N-1, N-1)` and back to `(0, 0)` by following the rules below:

1. From a cell `(i, j)`, you can walk to cell `(i + 1, j)` or `(i, j + 1)`.
2. After reaching `(N-1, N-1)`, retrace steps by walking to `(i-1, j)` or `(i, j-1)`.
3. Can pass through a cell multiple times, but only pick cherries once from each cell.

### Constraints:
- `N == grid.length`
- `N == grid[i].length`
- `1 <= N <= 50`
- `grid[i][j]` is `-1`, `0`, or `1`.

---

### [Approach 1: Naive Recursive Solution](#approach-1)

#### Intuition:
A straightforward way to solve the problem is by using a naive recursive approach. The problem seems like a combination of two pathfinding problems. However, this approach will not work due to overlapping subproblems and exponential time complexity. It is presented here as a stepping stone towards better solutions.

#### Code:

```typescript
function cherryPickup(grid: number[][]): number {
    const N = grid.length;
    
    function dfs(r1: number, c1: number, r2: number, c2: number): number {
        if (r1 >= N || c1 >= N || r2 >= N || c2 >= N ||
            grid[r1][c1] == -1 || grid[r2][c2] == -1) {
            return -Infinity;
        }

        // Base case: both on the same cell and reached end cell
        if (r1 == N-1 && c1 == N-1) {
            return grid[r1][c1];
        }

        // Collect cherries
        let result = grid[r1][c1];
        if (r1 != r2 || c1 != c2) {
            result += grid[r2][c2];
        }

        // Maximize the cherries collected
        result += Math.max(
            dfs(r1+1, c1, r2+1, c2), // both move down
            dfs(r1, c1+1, r2, c2+1), // both move right
            dfs(r1+1, c1, r2, c2+1), // first moves down, second moves right
            dfs(r1, c1+1, r2+1, c2)  // first moves right, second moves down
        );

        return result;
    }

    return Math.max(0, dfs(0, 0, 0, 0));
}
```

#### Time Complexity:
- **Exponential** due to the vast number of calls for even moderate values of `N`.

#### Space Complexity:
- **O(N^2)** stack space in implicit recursion.

---

### [Approach 2: Dynamic Programming - 3D DP](#approach-2)

#### Intuition:
To optimize, use dynamic programming to avoid recalculating overlapping subproblems. We can use a 3D DP array. `dp[r1][c1][r2]` will store the maximum cherries collected where `r1, c1` are coordinates of the first path and `r2` for the second path. This utilizes the fact that if both have taken `k` steps, then `c2` can be derived from `r2` and `k`: `r2 + c2 = r1 + c1`.

#### Code:

```typescript
function cherryPickup(grid: number[][]): number {
    const N = grid.length;
    const dp: number[][][] = Array.from({ length: N }, () =>
        Array.from({ length: N }, () => Array(N).fill(-Infinity))
    );
    
    dp[0][0][0] = grid[0][0];

    for (let k = 1; k < 2 * N - 1; ++k) {
        for (let r1 = Math.min(k, N - 1); r1 >= 0; --r1) {
            for (let r2 = Math.min(k, N - 1); r2 >= 0; --r2) {
                const c1 = k - r1;
                const c2 = k - r2;
                if (c1 < 0 || c1 >= N || c2 < 0 || c2 >= N || 
                    grid[r1][c1] == -1 || grid[r2][c2] == -1) {
                    continue;
                }

                let result = dp[r1][r2][k - 1];
                if (r1 > 0) result = Math.max(result, dp[r1 - 1][r2][k - 1]);
                if (r2 > 0) result = Math.max(result, dp[r1][r2 - 1][k - 1]);
                if (r1 > 0 && r2 > 0) result = Math.max(result, dp[r1 - 1][r2 - 1][k - 1]);

                result += grid[r1][c1];
                if (r1 != r2) {
                    result += grid[r2][c2];
                }
                
                dp[r1][r2][k] = result;
            }
        }
    }

    return Math.max(0, dp[N-1][N-1][2*N-2]);
}
```

#### Time Complexity:
- **O(N^3)** as we iterate over the possible values of `r1`, `r2`, and `k`.

#### Space Complexity:
- **O(N^3)** due to the 3D dp array.

---

### [Approach 3: Dynamic Programming - Optimized 2D DP](#approach-3)

#### Intuition:
This can be further optimized by understanding that `c1` and `c2` can be derived from the number of steps `k` and the indices `r1` and `r2`. We can simplify the DP to use only two dimensions by deriving `c1` and `c2`.

#### Code:

```typescript
function cherryPickup(grid: number[][]): number {
    const N = grid.length;
    const dp: number[][] = Array.from({ length: N }, () => Array(N).fill(-Infinity));

    dp[0][0] = grid[0][0];

    for (let k = 1; k < 2 * N - 1; k++) {
        let currentDP: number[][] = Array.from({ length: N }, () => Array(N).fill(-Infinity));
        for (let r1 = Math.min(N - 1, k); r1 >= 0; r1--) {
            for (let r2 = Math.min(N - 1, k); r2 >= 0; r2--) {
                const c1 = k - r1;
                const c2 = k - r2;
                if (c1 >= N || c2 >= N || grid[r1][c1] === -1 || grid[r2][c2] === -1) {
                    continue;
                }

                let value = dp[r1][r2];
                if (r1 > 0) value = Math.max(value, dp[r1 - 1][r2]);
                if (r2 > 0) value = Math.max(value, dp[r1][r2 - 1]);
                if (r1 > 0 && r2 > 0) value = Math.max(value, dp[r1 - 1][r2 - 1]);
                
                value += grid[r1][c1];
                if (r1 !== r2) {
                    value += grid[r2][c2];
                }
                
                currentDP[r1][r2] = value;
            }
        }
        dp = currentDP;
    }

    return Math.max(0, dp[N - 1][N - 1]);
}
```

#### Time Complexity:
- **O(N^3)**, same reasoning as the 3D DP approach.

#### Space Complexity:
- **O(N^2)**, since only two 2D arrays of size `N x N` are used, one for the current state and one for the previous state.

