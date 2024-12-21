## [Leetcode Problem 741: Cherry Pickup](https://leetcode.com/problems/cherry-pickup/)

### Approaches

1. [Recursive Approach with Memoization](#recursive-approach-with-memoization)
2. [Dynamic Programming (Bottom-Up) Approach](#dynamic-programming-bottom-up-approach)

---

### Recursive Approach with Memoization

#### Intuition

The problem is essentially a variant of the "grid path finding" with constraints and requires backtracking. We can view the problem as two paths (from top-left to bottom-right and then back from bottom-right to top-left) collecting cherries, where path intersections can contribute cherries only once.

A recursive approach can explore the maximum cherries we can collect for a given position in the grid when both paths are considered. Using memoization helps store results of overlapping sub-problems to avoid redundant calculations.

#### Key Steps in Code

1. Create a 3D `memo` array to store intermediate results.
2. Implement a recursive function `dp` which takes `(r1, c1, r2)` as its parameters, where `(r1, c1)` is the current position of path 1, and `(r2, c2)` is the current position of path two. `c2` can be derived as `c1 + r1 - r2` due to identical-time movement.
3. Check if positions are valid (within bounds and not on thorn (`-1`)).
4. Use memoized results if available.
5. Use recursive calls to explore all possible moves for both persons.
6. Sum cherries from both paths, avoid double-counting at intersections.
7. Return result after marking it in `memo`.

#### Code

```javascript
var cherryPickup = function(grid) {
    const n = grid.length;
    const memo = Array.from({ length: n }, () => 
                Array.from({ length: n }, () => 
                Array(n).fill(null)));
    
    const dp = (r1, c1, r2) => {
        const c2 = r1 + c1 - r2;
        if (r1 >= n || c1 >= n || r2 >= n || c2 >= n || 
            grid[r1][c1] === -1 || grid[r2][c2] === -1) {
            return -Infinity;
        }
        if (r1 === n - 1 && c1 === n - 1) {
            return grid[r1][c1];
        }
        
        if (memo[r1][c1][r2] !== null) {
            return memo[r1][c1][r2];
        }
        
        let result = grid[r1][c1];
        if (r1 !== r2) {
            result += grid[r2][c2];
        }
        
        result += Math.max(
            dp(r1 + 1, c1, r2 + 1),  // both down
            dp(r1, c1 + 1, r2 + 1),  // right-down
            dp(r1 + 1, c1, r2),      // down-right
            dp(r1, c1 + 1, r2)       // both right
        );
        
        memo[r1][c1][r2] = result;
        return result;
    };
    
    return Math.max(0, dp(0, 0, 0));
};
```

#### Time Complexity

- The recursion has a time complexity of \(O(N^3)\), where \(N\) is the dimension of the grid because three variables (r1, c1, and r2) each have a possible range of values up to \(N\).

#### Space Complexity

- Space complexity is \(O(N^3)\) due to the memoization table storing results for each sub-problem combination.

---

### Dynamic Programming (Bottom-Up) Approach

#### Intuition

A bottom-up DP approach can avoid the explicit recursion by iteratively solving for maximum cherries collected in sub-problems by examining all paths that persons could have taken concurrently.

#### Key Steps in Code

1. Initialize a 3D DP array.
2. Populate base cases for initial positions.
3. Iterate through potential grid positions for both paths and fill the DP table using past computed states.
4. Transition logic must combine previous possible states in typical DP fashion.

#### Code

```javascript
var cherryPickup = function(grid) {
    const n = grid.length;
    const dp = Array.from({ length: n }, () => 
               Array.from({ length: n }, () => 
               Array(n).fill(-Infinity)));
    
    dp[0][0][0] = grid[0][0];
    
    for (let r1 = 0; r1 < n; ++r1) {
        for (let c1 = 0; c1 < n; ++c1) {
            for (let r2 = 0; r2 < n; ++r2) {
                const c2 = r1 + c1 - r2;
                if (c2 < 0 || c2 >= n || grid[r1][c1] == -1 || grid[r2][c2] == -1) {
                    continue;
                }
                
                let cherries = grid[r1][c1];
                if (r1 !== r2) {
                    cherries += grid[r2][c2];
                }
                
                if (r1 > 0 && r2 > 0) {
                    dp[r1][c1][r2] = Math.max(dp[r1][c1][r2], dp[r1 - 1][c1][r2 - 1] + cherries);
                }
                if (r1 > 0 && c2 > 0) {
                    dp[r1][c1][r2] = Math.max(dp[r1][c1][r2], dp[r1 - 1][c1][r2] + cherries);
                }
                if (c1 > 0 && r2 > 0) {
                    dp[r1][c1][r2] = Math.max(dp[r1][c1][r2], dp[r1][c1 - 1][r2 - 1] + cherries);
                }
                if (c1 > 0 && c2 > 0) {
                    dp[r1][c1][r2] = Math.max(dp[r1][c1][r2], dp[r1][c1 - 1][r2] + cherries);
                }
            }
        }
    }
    
    return Math.max(0, dp[n - 1][n - 1][n - 1]);
};
```

#### Time Complexity

- Time complexity is \(O(N^3)\) due to iterating over n possible values for r1, c1, and r2.

#### Space Complexity

- Space complexity is \(O(N^3)\) because of the 3D table storing results for all combinations of given states.

---

The above solutions provide robust ways to approach the Cherry Pickup problem using recursive memoization and dynamic programming, achieving efficient optimal solutions to collect the maximum cherries possible.

