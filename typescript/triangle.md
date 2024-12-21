# [Leetcode 120: Triangle](https://leetcode.com/problems/triangle/)

## Approaches
- [Approach 1: Recursive Approach](#approach-1)
- [Approach 2: Memoization (Top-Down DP)](#approach-2)
- [Approach 3: Dynamic Programming (Bottom-Up DP) with additional space](#approach-3)
- [Approach 4: Optimized Dynamic Programming (Bottom-Up DP) without additional space](#approach-4)

## Approach 1: Recursive Approach

### Intuition
The problem can naturally be expressed recursively. Starting at the top of the triangle, at each position `(i, j)`, the path splits into two options: going directly down to `(i + 1, j)` or diagonally down-right to `(i + 1, j + 1)`. The final minimum path sum can be obtained by choosing the path that gives the minimum sum recursively.

### Code
```typescript
function minimumTotal(triangle: number[][]): number {
    function dfs(i: number, j: number): number {
        // Base case: when we reach the bottom row
        if (i === triangle.length - 1) {
            return triangle[i][j];
        }
        // Recursive exploration of paths
        const down = dfs(i + 1, j);
        const downRight = dfs(i + 1, j + 1);
        return triangle[i][j] + Math.min(down, downRight);
    }
    
    // Start from the top
    return dfs(0, 0);
}
```

### Complexity
- **Time Complexity:** O(2^n), where n is the number of rows in the triangle. Each position in the triangle can split into two recursive calls.
- **Space Complexity:** O(n), considering the recursive stack space.

## Approach 2: Memoization (Top-Down DP)

### Intuition
The recursive approach involves many repeated calculations. To optimize, we can store results of subproblems and reuse them when needed, significantly reducing the number of calculations.

### Code
```typescript
function minimumTotal(triangle: number[][]): number {
    const memo: number[][] = Array.from({ length: triangle.length }, () => new Array(triangle.length).fill(undefined));

    function dfs(i: number, j: number): number {
        if (i === triangle.length - 1) {
            return triangle[i][j];
        }
        if (memo[i][j] !== undefined) {
            return memo[i][j];
        }
        const down = dfs(i + 1, j);
        const downRight = dfs(i + 1, j + 1);
        memo[i][j] = triangle[i][j] + Math.min(down, downRight);
        return memo[i][j];
    }

    return dfs(0, 0);
}
```

### Complexity
- **Time Complexity:** O(n^2), as each subproblem is solved once.
- **Space Complexity:** O(n^2) for memoization storage.

## Approach 3: Dynamic Programming (Bottom-Up DP) with additional space

### Intuition
By building a DP table from the bottom of the triangle upwards, we can iteratively calculate the minimum path sum, starting from the last row and working towards the top. This eliminates the need for recursion and significantly improves efficiency.

### Code
```typescript
function minimumTotal(triangle: number[][]): number {
    const n = triangle.length;
    const dp = Array.from({ length: n }, (_, i) => [...triangle[i]]);
    
    // Iterating from the second last row to the top
    for (let i = n - 2; i >= 0; i--) {
        for (let j = 0; j <= i; j++) {
            // Updating dp table with min path sums calculated upwards
            dp[i][j] += Math.min(dp[i + 1][j], dp[i + 1][j + 1]);
        }
    }
    
    return dp[0][0]; // The top of the triangle
}
```

### Complexity
- **Time Complexity:** O(n^2), we process each item in the triangle exactly once.
- **Space Complexity:** O(n^2), due to the extra DP table used.

## Approach 4: Optimized Dynamic Programming (Bottom-Up DP) without additional space

### Intuition
We can further optimize our space usage by updating the input triangle itself to store the computed minimum paths, rather than using an additional DP array. This approach is more memory efficient, especially for large triangles.

### Code
```typescript
function minimumTotal(triangle: number[][]): number {
    const n = triangle.length;

    // Start from the bottom of the triangle updating values
    for (let i = n - 2; i >= 0; i--) {
        for (let j = 0; j <= i; j++) {
            triangle[i][j] += Math.min(triangle[i + 1][j], triangle[i + 1][j + 1]);
        }
    }
    
    return triangle[0][0]; // The result is at the top of the triangle
}
```

### Complexity
- **Time Complexity:** O(n^2), similar reasoning as previous approach.
- **Space Complexity:** O(1), reusing the input triangle array for DP.

