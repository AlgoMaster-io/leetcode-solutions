# [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)

## Approaches
1. [Breadth-First Search (BFS)](#bfs-approach)
2. [Depth-First Search (DFS)](#dfs-approach)

---

### BFS Approach

#### Intuition:
The problem requires us to find all the grid positions where water can flow to both the Pacific and Atlantic oceans. We can approach this using BFS by treating each ocean separately:
1. Treat each grid cell adjacent to the Pacific Ocean (top and left edges) as a starting point.
2. Similarly, treat each grid cell adjacent to the Atlantic Ocean (bottom and right edges) as a starting point.
3. From these edges, perform a BFS to mark all cells that can be reached by water flowing downhill.
4. The intersection of cells reachable by both BFS operations will give the required positions.

#### Steps:
1. Use two boolean matrices to track cell reachability for both Pacific and Atlantic.
2. Conduct BFS from the Pacific edge and mark all reachable cells starting from there.
3. Similarly, conduct BFS from the Atlantic edge and mark all reachable cells.
4. The result consists of cells marked as reachable by both Pacific and Atlantic oceans.

#### Time and Space Complexity:
- **Time Complexity**: O(m * n), where m and n are the dimensions of the matrix because each cell is processed at most twice.
- **Space Complexity**: O(m * n) for the queues and visited matrices.

```javascript
function pacificAtlantic(matrix) {
    if (!matrix || matrix.length === 0 || matrix[0].length === 0) return [];

    const m = matrix.length;
    const n = matrix[0].length;
    const pacificReachable = Array.from({ length: m }, () => Array(n).fill(false));
    const atlanticReachable = Array.from({ length: m }, () => Array(n).fill(false));

    function bfs(starts, reachable) {
        const queue = starts.slice();
        while (queue.length) {
            const [x, y] = queue.shift();
            reachable[x][y] = true;

            // Check all four directions
            for (const [dx, dy] of [[0, 1], [1, 0], [0, -1], [-1, 0]]) {
                const nx = x + dx;
                const ny = y + dy;
                if (
                    nx >= 0 && ny >= 0 && nx < m && ny < n && 
                    !reachable[nx][ny] && 
                    matrix[nx][ny] >= matrix[x][y]
                ) {
                    queue.push([nx, ny]);
                }
            }
        }
    }

    // Initial cells that can reach the Pacific and Atlantic
    const pacificStarts = [];
    const atlanticStarts = [];
    for (let i = 0; i < m; i++) {
        pacificStarts.push([i, 0]);
        atlanticStarts.push([i, n - 1]);
    }
    for (let i = 0; i < n; i++) {
        pacificStarts.push([0, i]);
        atlanticStarts.push([m - 1, i]);
    }

    bfs(pacificStarts, pacificReachable);
    bfs(atlanticStarts, atlanticReachable);

    // Find cells that are reachable by both oceans
    const result = [];
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (pacificReachable[i][j] && atlanticReachable[i][j]) {
                result.push([i, j]);
            }
        }
    }
    return result;
}
```

---

### DFS Approach

#### Intuition:
Similarly to BFS, we can use DFS to explore all the cells starting from the ocean borders. The method is mostly the same:
1. Initialize two matrices to track cells reachable from each ocean.
2. Perform DFS to fill these matrices.
3. Collect the cells that are reachable from both oceans.

#### Steps:
1. Use two boolean matrices for Pacific and Atlantic as before.
2. Conduct DFS from the Pacific edge, marking reachable cells.
3. Conduct DFS from the Atlantic edge, marking reachable cells.
4. Find intersecting cells in both matrices.

#### Time and Space Complexity:
- **Time Complexity**: O(m * n), because each node in the graph is visited once per DFS initiated.
- **Space Complexity**: O(m * n) due to recursive stack space.

```javascript
function pacificAtlanticDFS(matrix) {
    if (!matrix || matrix.length === 0 || matrix[0].length === 0) return [];

    const m = matrix.length;
    const n = matrix[0].length;
    const pacificReachable = Array.from({ length: m }, () => Array(n).fill(false));
    const atlanticReachable = Array.from({ length: m }, () => Array(n).fill(false));

    function dfs(x, y, reachable) {
        reachable[x][y] = true;

        for (const [dx, dy] of [[0, 1], [1, 0], [0, -1], [-1, 0]]) {
            const nx = x + dx;
            const ny = y + dy;
            if (
                nx >= 0 && ny >= 0 && nx < m && ny < n &&
                !reachable[nx][ny] &&
                matrix[nx][ny] >= matrix[x][y]
            ) {
                dfs(nx, ny, reachable);
            }
        }
    }

    for (let i = 0; i < m; i++) {
        dfs(i, 0, pacificReachable);
        dfs(i, n - 1, atlanticReachable);
    }
    for (let i = 0; i < n; i++) {
        dfs(0, i, pacificReachable);
        dfs(m - 1, i, atlanticReachable);
    }

    const result = [];
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (pacificReachable[i][j] && atlanticReachable[i][j]) {
                result.push([i, j]);
            }
        }
    }
    return result;
}
```


