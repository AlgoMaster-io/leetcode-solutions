# [Leetcode 120: Triangle](https://leetcode.com/problems/triangle/)

## Approaches:
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Dynamic Programming - Top-Down Approach (Memoization)](#approach-2-dynamic-programming---top-down-approach-memoization)
- [Approach 3: Dynamic Programming - Bottom-Up Approach (Tabulation)](#approach-3-dynamic-programming---bottom-up-approach-tabulation)
- [Approach 4: Space Optimized Dynamic Programming](#approach-4-space-optimized-dynamic-programming)

### Approach 1: Recursive Solution

#### Intuition:
The triangle path problem can be imagined as visiting each node starting from the top and branching either directly below or to the bottom right. A recursive approach involves considering all paths from the top of the triangle to reach the base, calculating their sums, and selecting the minimum. This can lead, however, to recalculating the same subproblems multiple times, which ultimately makes it inefficient for larger triangles.

```javascript
function minimumTotal(triangle) {
    // Recursive function to explore all paths
    function recursive(row, col) {
        // Base case when we reach the last row
        if (row === triangle.length) {
            return 0;
        }

        // Recursive call for the path directly below
        let down = recursive(row + 1, col);
        
        // Recursive call for the path diagonally right
        let right = recursive(row + 1, col + 1);
        
        // Return the minimum path sum of current position
        return triangle[row][col] + Math.min(down, right);
    }

    return recursive(0, 0);
}
```

- **Time Complexity:** O(2^n), because each node branches into two paths.
- **Space Complexity:** O(n) due to recursive call stack.

### Approach 2: Dynamic Programming - Top-Down Approach (Memoization)

#### Intuition:
Memoization enhances the recursive approach by storing results of similar subproblems encountered during recursion, reducing redundant computations.

```javascript
function minimumTotal(triangle) {
    const memo = Array(triangle.length).fill().map(() => Array());

    function recursive(row, col) {
        if (row === triangle.length) {
            return 0;
        }

        if (memo[row][col] !== undefined) {
            return memo[row][col];
        }

        let down = recursive(row + 1, col);
        let right = recursive(row + 1, col + 1);
        memo[row][col] = triangle[row][col] + Math.min(down, right);

        return memo[row][col];
    }

    return recursive(0, 0);
}
```

- **Time Complexity:** O(n^2), as each element is computed once.
- **Space Complexity:** O(n^2) for storing intermediate results.

### Approach 3: Dynamic Programming - Bottom-Up Approach (Tabulation)

#### Intuition:
In tabulation, start from the base of the triangle and move upwards, calculating and storing results in-place.

```javascript
function minimumTotal(triangle) {
    let n = triangle.length;
    let dp = triangle[n - 1].slice();  // Start with the last row

    for (let row = n - 2; row >= 0; row--) {
        for (let col = 0; col <= row; col++) {
            // Choose the minimum path sum from below or diagonally below
            dp[col] = triangle[row][col] + Math.min(dp[col], dp[col + 1]);
        }
    }

    return dp[0];
}
```

- **Time Complexity:** O(n^2), iterating through each element once.
- **Space Complexity:** O(n) using one-dimensional array for dynamic programming.

### Approach 4: Space Optimized Dynamic Programming

#### Intuition:
Utilize the fact that the previous row is no longer needed once the next row is computed. Thus, the `dp` array acts like a sliding window across rows, reducing space usage.

```javascript
function minimumTotal(triangle) {
    let n = triangle.length;
    
    // Start from the last row
    let dp = triangle[n - 1].slice();

    for (let row = n - 2; row >= 0; row--) {
        for (let col = 0; col <= row; col++) {
            dp[col] = triangle[row][col] + Math.min(dp[col], dp[col + 1]);
        }
    }

    return dp[0];
}
```

- **Time Complexity:** O(n^2), iterating through each element.
- **Space Complexity:** O(n), as we use only a flat array to store the current state.

