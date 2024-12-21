# [Leetcode 741: Cherry Pickup](https://leetcode.com/problems/cherry-pickup/)

## Approaches
- [Approach 1: Recursive DFS with Memoization](#approach-1-recursive-dfs-with-memoization)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

### Approach 1: Recursive DFS with Memoization

#### Intuition
The idea is to use recursion to simulate the journey of moving from (0,0) to (n-1,n-1) and back again, picking cherries on the way. By memoizing the results of subproblems, we avoid redundant calculations.

#### Steps
1. We define dp(r1, c1, r2): how many cherries we can pick if one journey travels from (r1, c1) to (n-1, n-1) and the other from (r2, c2) to (n-1, n-1).
2. Notice that (r2, c2) is dependent on the steps taken: r2 + c2 = r1 + c1.
3. If either position is out of bounds or on a thorn, return -1 representing this path is not feasible.
4. Calculate `cherries` by summing the cherries at both positions.
5. Explore potential subproblems (`4` possible directions) and recursively calculate the maximum cherries picked.
6. Use memoization to store intermediate results.

```go
func cherryPickup(grid [][]int) int {
    n := len(grid)
    memo := make(map[[3]int]int)

    var dp func(r1, c1, r2 int) int
    dp = func(r1, c1, r2 int) int {
        c2 := r1 + c1 - r2
        if r1 >= n || c1 >= n || r2 >= n || c2 >= n || grid[r1][c1] == -1 || grid[r2][c2] == -1 {
            return -1
        }
        if r1 == n-1 && c1 == n-1 {
            return grid[r1][c1]
        }
        if val, ok := memo[[3]int{r1, c1, r2}]; ok {
            return val
        }
        res := grid[r1][c1]
        if r1 != r2 {
            res += grid[r2][c2]
        }
        temp := max(
            max(dp(r1+1, c1, r2+1), dp(r1+1, c1, r2)),
            max(dp(r1, c1+1, r2+1), dp(r1, c1+1, r2)),
        )
        if temp != -1 {
            res += temp
        } else {
            res = -1
        }
        memo[[3]int{r1, c1, r2}] = res
        return res
    }

    result := dp(0, 0, 0)
    if result == -1 {
        return 0
    }
    return result
}
```

**Time Complexity:** O(n^3) since there are O(n^2) possible state pairs and for each state up to 4 recursive calls are considered.  
**Space Complexity:** O(n^3) due to memoization storage.

### Approach 2: Dynamic Programming

#### Intuition
Rather than recalculating values recursively, we can use bottom-up dynamic programming to build the solution iteratively. The idea here is to fill a dp table where each cell represents the maximum cherries collected by two paths starting at (0,0) to respective points in the grid.

#### Steps
1. Define dp[k][r1][r2], which is the maximum cherries collected when the first traveler reaches (r1,c1) and the second, reaches (r2,c2) after k steps.
2. Iterate from k step 0 to 2*(n-1) where n is grid length, since the longest path requires 2*(n-1) steps.
3. For each k, try all valid coordinates (r1, r2) that satisfy (r1+c1) = (r2+c2) = k.
4. Update the dp table by considering all possible moves: right or down for both travelers.
5. Finally, dp at (n-1,n-1) gives the result.

```go
func cherryPickup(grid [][]int) int {
    n := len(grid)
    dp := make([][][]int, 2*n-1)
    for k := range dp {
        dp[k] = make([][]int, n)
        for i := range dp[k] {
            dp[k][i] = make([]int, n)
            for j := range dp[k][i] {
                dp[k][i][j] = -1
            }
        }
    }
    dp[0][0][0] = grid[0][0]

    for k := 1; k < 2*n-1; k++ {
        for r1 := max(0, k-(n-1)); r1 <= min(n-1, k); r1++ {
            for r2 := max(0, k-(n-1)); r2 <= min(n-1, k); r2++ {
                c1, c2 := k-r1, k-r2
                if c1 < 0 || c1 >= n || c2 < 0 || c2 >= n || grid[r1][c1] == -1 || grid[r2][c2] == -1 {
                    continue
                }
                cherries := grid[r1][c1]
                if r1 != r2 || c1 != c2 {
                    cherries += grid[r2][c2]
                }
                maxCherries := -1
                for _, dr1 := range []int{-1, 0} {
                    for _, dr2 := range []int{-1, 0} {
                        rr1, rr2 := r1+dr1, r2+dr2
                        if rr1 >= 0 && rr2 >= 0 {
                            maxCherries = max(maxCherries, dp[k-1][rr1][rr2])
                        }
                    }
                }
                if maxCherries != -1 {
                    dp[k][r1][r2] = maxCherries + cherries
                }
            }
        }
    }
    return max(0, dp[2*n-2][n-1][n-1])
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

**Time Complexity:** O(n^3) due to iterating over all possible states up and down two times.  
**Space Complexity:** O(n^3) for holding the dp table.

