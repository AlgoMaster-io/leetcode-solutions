# 329. [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approach 1: DFS with Memoization

### Solution
```javascript
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
class Solution {
    constructor() {
        this.directions = [[0, 1], [1, 0], [0, -1], [-1, 0]]; // 4 possible directions: right, down, left, up
    }

    longestIncreasingPath(matrix) {
        if (!matrix || matrix.length === 0 || matrix[0].length === 0) return 0;
        const m = matrix.length, n = matrix[0].length;
        const memo = Array.from({ length: m }, () => Array(n).fill(0)); // Memoization array to store results of subproblems
        let longestPath = 0;

        // Explore each cell in the matrix using DFS
        for (let i = 0; i < m; i++) {
            for (let j = 0; j < n; j++) {
                longestPath = Math.max(longestPath, this.dfs(matrix, i, j, memo));
            }
        }

        return longestPath;
    }

    dfs(matrix, i, j, memo) {
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
```javascript
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
class Solution {
    constructor() {
        this.directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];
    }

    longestIncreasingPath(matrix) {
        if (!matrix || matrix.length === 0 || matrix[0].length === 0) return 0;
        const m = matrix.length, n = matrix[0].length;
        const indegree = Array.from({ length: m }, () => Array(n).fill(0)); // Array to store number of incoming edges

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

        const queue = [];
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
                const [i, j] = queue.shift();
                for (const dir of this.directions) {
                    const x = i + dir[0], y = j + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[i][j]) {
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

