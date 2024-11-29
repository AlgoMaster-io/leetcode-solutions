# 329. [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approach 1: DFS with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)

class Solution {
    private directions: number[][] = [[0, 1], [1, 0], [0, -1], [-1, 0]]; // 4 possible directions: right, down, left, up

    public longestIncreasingPath(matrix: number[][]): number {
        if (matrix == null || matrix.length === 0 || matrix[0].length === 0) return 0;
        const m = matrix.length, n = matrix[0].length;
        const memo: number[][] = Array.from({ length: m }, () => Array(n).fill(0)); // Memoization array to store results of subproblems
        let longestPath = 0;

        // Explore each cell in the matrix using DFS
        for (let i = 0; i < m; i++) {
            for (let j = 0; j < n; j++) {
                longestPath = Math.max(longestPath, this.dfs(matrix, i, j, memo));
            }
        }

        return longestPath;
    }

    private dfs(matrix: number[][], i: number, j: number, memo: number[][]): number {
        if (memo[i][j] !== 0) return memo[i][j]; // Return cached result if already computed

        let maxPath = 1; // Minimum path length is 1 (the cell itself)

        // Explore all four directions
        for (const dir of this.directions) {
            const x = i + dir[0], y = j + dir[1];
            if (x >= 0 && x < matrix.length && y >= 0 && y < matrix[0].length && matrix[x][y] > matrix[i][j]) {
                const length = 1 + this.dfs(matrix, x, y, memo);
                maxPath = Math.max(maxPath, length);
            }
        }

        memo[i][j] = maxPath; // Cache the result before returning

        return maxPath;
    }
}
```

## Approach 2: Topological Sort with Indegree

### Solution
typescript
```typescript
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)

class Solution {
    private directions: number[][] = [[0, 1], [1, 0], [0, -1], [-1, 0]];

    public longestIncreasingPath(matrix: number[][]): number {
        if (matrix == null || matrix.length === 0 || matrix[0].length === 0) return 0;
        const m = matrix.length, n = matrix[0].length;
        const indegree: number[][] = Array.from({ length: m }, () => Array(n).fill(0)); // Array to store number of incoming edges

        // Calculate indegrees of each cell
        for (let i = 0; i < m; i++) {
            for (let j = 0; j < n; j++) {
                for (const dir of this.directions) {
                    const x = i + dir[0], y = j + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[i][j]) {
                        indegree[x][y]++;
                    }
                }
            }
        }

        const queue: number[][] = [];
        // Initialize queue with cells having 0 indegree
        for (let i = 0; i < m; i++) {
            for (let j = 0; j < n; j++) {
                if (indegree[i][j] === 0) {
                    queue.push([i, j]);
                }
            }
        }

        let pathLength = 0;
        // Process each cell in the queue
        while (queue.length > 0) {
            const size = queue.length;
            pathLength++;
            for (let k = 0; k < size; k++) {
                const cell = queue.shift()!;
                for (const dir of this.directions) {
                    const x = cell[0] + dir[0], y = cell[1] + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[cell[0]][cell[1]]) {
                        indegree[x][y]--;
                        if (indegree[x][y] === 0) {
                            queue.push([x, y]);
                        }
                    }
                }
            }
        }

        return pathLength;
    }
}
```

