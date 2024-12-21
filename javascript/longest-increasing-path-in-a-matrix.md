# [Leetcode 329: Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approaches
- [Brute Force with DFS](#brute-force-with-dfs)
- [Optimized DFS with Memoization](#optimized-dfs-with-memoization)
- [Topological Sort using Indegree Count](#topological-sort-using-indegree-count)

---

### Brute Force with DFS

#### Intuition
The brute force approach is to start a Depth-First Search (DFS) traversal from each cell and try to move in any of the four possible directions (up, down, left, right) if the next cell has a greater value. The maximum path length from any starting cell will give us the longest increasing path in the matrix. This approach, however, will involve recalculating paths multiple times, leading to a Time Limit Exceeded error on larger inputs.

#### Code
```javascript
function longestIncreasingPath(matrix) {
    if (!matrix || !matrix.length || !matrix[0].length) return 0;

    const rows = matrix.length;
    const cols = matrix[0].length;
    let maxPath = 0;

    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];

    function dfs(r, c, prevValue) {
        // Base case: if the out of bounds or not increasing
        if (r < 0 || c < 0 || r >= rows || c >= cols || matrix[r][c] <= prevValue) return 0;

        let length = 0;
        for (const [dr, dc] of directions) {
            length = Math.max(length, dfs(r + dr, c + dc, matrix[r][c]));
        }
        return 1 + length;
    }

    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            maxPath = Math.max(maxPath, dfs(r, c, -1));
        }
    }

    return maxPath;
}
```

#### Time Complexity
- **Time Complexity:** O((m * n) * 4^(m*n)) where m is the number of rows and n is the number of columns.
- **Space Complexity:** O(m*n) considering recursive stack space.

---

### Optimized DFS with Memoization

#### Intuition
To optimize the solution, we can use memoization to store the length of the longest increasing path starting from each cell. This will help us to avoid recomputation and give us an efficient solution. If at any point, we already know the result from a cell, we will simply use the cached result instead of recalculating.

#### Code
```javascript
function longestIncreasingPath(matrix) {
    if (!matrix || !matrix.length || !matrix[0].length) return 0;

    const rows = matrix.length;
    const cols = matrix[0].length;
    const cache = Array.from({ length: rows }, () => Array(cols).fill(null));
    let maxPath = 0;

    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];

    function dfs(r, c, prevValue) {
        if (r < 0 || c < 0 || r >= rows || c >= cols || matrix[r][c] <= prevValue) return 0;
        if (cache[r][c] !== null) return cache[r][c];

        let length = 0;
        for (const [dr, dc] of directions) {
            length = Math.max(length, dfs(r + dr, c + dc, matrix[r][c]));
        }
        cache[r][c] = 1 + length;
        return cache[r][c];
    }

    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            maxPath = Math.max(maxPath, dfs(r, c, -1));
        }
    }

    return maxPath;
}
```

#### Time Complexity
- **Time Complexity:** O(m*n) since each cell is computed only once.
- **Space Complexity:** O(m*n) due to memoization and recursive stack space.

---

### Topological Sort using Indegree Count

#### Intuition
Another optimal approach is to use topological sort. We can transform the problem into a graph problem where each cell is a node and edges point from a cell to any neighboring cells with greater values. The longest increasing path is equivalent to the largest number of edges in any path. We can use the idea of topological sorting and calculate the indegree for cells. We continue processing cells starting with zero indegree in layers until we can't proceed further.

#### Code
```javascript
function longestIncreasingPath(matrix) {
    if (!matrix || !matrix.length || !matrix[0].length) return 0;

    const rows = matrix.length;
    const cols = matrix[0].length;
    const indegree = Array.from({ length: rows }, () => Array(cols).fill(0));
    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];

    // Calculate the indegree of each cell
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            directions.forEach(([dr, dc]) => {
                const newR = r + dr;
                const newC = c + dc;
                if (
                    newR >= 0 && newC >= 0 && newR < rows && newC < cols &&
                    matrix[newR][newC] > matrix[r][c]
                ) {
                    indegree[newR][newC]++;
                }
            });
        }
    }

    const queue = [];
    // Enqueue all cells with indegree zero
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (indegree[r][c] === 0) {
                queue.push([r, c]);
            }
        }
    }

    let maxLength = 0;
    while (queue.length) {
        const size = queue.length;
        maxLength++;
        for (let i = 0; i < size; i++) {
            const [r, c] = queue.shift();

            directions.forEach(([dr, dc]) => {
                const newR = r + dr;
                const newC = c + dc;
                if (
                    newR >= 0 && newC >= 0 && newR < rows && newC < cols &&
                    matrix[newR][newC] > matrix[r][c]
                ) {
                    indegree[newR][newC]--;
                    if (indegree[newR][newC] === 0) {
                        queue.push([newR, newC]);
                    }
                }
            });
        }
    }

    return maxLength;
}
```

#### Time Complexity
- **Time Complexity:** O(m*n) as each cell and each edge is processed once.
- **Space Complexity:** O(m*n) to maintain the indegree matrix and queue.

This method effectively finds the longest path by processing the entire matrix as a graph and collapsing nodes based on indegree, which ensures we efficiently process only possible augmenting paths.

