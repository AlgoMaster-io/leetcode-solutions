# [Leetcode 200: Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Table of Contents
1. [Approach 1: Depth-First Search (DFS)](#approach-1-depth-first-search-dfs)
2. [Approach 2: Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)
3. [Approach 3: Union-Find (Disjoint Set Union)](#approach-3-union-find-disjoint-set-union)

### Approach 1: Depth-First Search (DFS)

#### Intuition
The intuition behind using DFS for this problem is that we can treat the grid as a graph where each cell is a node and each neighbouring cell (upwards, downwards, left, right) is connected if they are both '1's. To count the number of islands, we can iterate over each cell, and for each '1', start a DFS to mark all connected '1's as visited. This ensures that each island is counted only once.

#### Steps
1. Initialize a counter for islands.
2. Iterate over each cell in the grid.
3. When '1' is found, increment the island counter and start a DFS from that cell to mark all connected '1's as visited.
4. In DFS, change the '1' to '0' or any visited marker to prevent re-visiting.
5. Continue this process until all cells have been processed.

```javascript
var numIslands = function(grid) {
    if (!grid || grid.length === 0) return 0;

    let numIslands = 0;

    const dfs = (i, j) => {
        // Check for bounds and water
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] === '0') {
            return;
        }

        // Mark the cell as visited
        grid[i][j] = '0';

        // Visit all adjacent neighbors
        dfs(i - 1, j);
        dfs(i + 1, j);
        dfs(i, j - 1);
        dfs(i, j + 1);
    };

    const numRows = grid.length;
    const numCols = grid[0].length;

    for (let i = 0; i < numRows; i++) {
        for (let j = 0; j < numCols; j++) {
            if (grid[i][j] === '1') {
                numIslands++;
                dfs(i, j);
            }
        }
    }

    return numIslands;
};
```

#### Complexity Analysis
- **Time Complexity**: O(M * N) where M is the number of rows and N is the number of columns. Each cell is visited once.
- **Space Complexity**: O(M * N) in case the grid is full of lands, and the depth of DFS call stack can be M * N.

---

### Approach 2: Breadth-First Search (BFS)

#### Intuition
BFS is another way to explore the grid. Similar to DFS, we'll iteratively explore all connected '1's starting from each unvisited '1', but instead of recursion, we'll use a queue to perform level-order traversal of connected parts.

#### Steps
1. Initialize a counter for islands.
2. Iterate over each cell in the grid.
3. When '1' is found, increment the island counter and start a BFS from that cell.
4. Use a queue to keep track of cells to visit, marking visited cells as '0'.
5. Continue this process until all cells have been processed.

```javascript
var numIslands = function(grid) {
    if (!grid || grid.length === 0) return 0;

    let numIslands = 0;
    const directions = [[-1, 0], [1, 0], [0, -1], [0, 1]]; // Up, Down, Left, Right

    const bfs = (i, j) => {
        const queue = [];
        queue.push([i, j]);
        grid[i][j] = '0';

        while (queue.length > 0) {
            const [x, y] = queue.shift();
            for (const [dx, dy] of directions) {
                const newX = x + dx;
                const newY = y + dy;
                if (newX >= 0 && newX < grid.length && newY >= 0 && newY < grid[0].length && grid[newX][newY] === '1') {
                    queue.push([newX, newY]);
                    grid[newX][newY] = '0';
                }
            }
        }
    };

    const numRows = grid.length;
    const numCols = grid[0].length;

    for (let i = 0; i < numRows; i++) {
        for (let j = 0; j < numCols; j++) {
            if (grid[i][j] === '1') {
                numIslands++;
                bfs(i, j);
            }
        }
    }

    return numIslands;
};
```

#### Complexity Analysis
- **Time Complexity**: O(M * N) where M is the number of rows and N is the number of columns. Each cell is processed once.
- **Space Complexity**: O(min(M, N)) for the queue, though in the worst case it could be O(M * N) due to needing to store many nodes temporarily.

---

### Approach 3: Union-Find (Disjoint Set Union)

#### Intuition
The Union-Find algorithm can efficiently detect the connected components in the grid. Every cell is initially its own parent. For each '1', union it with its adjacent '1's, effectively connecting the grid into disjoint sets. The parent array will then have a unique representative for each island.

#### Steps
1. Initialize a union-find data structure for the grid.
2. Iterate over each cell, performing union operations with valid neighbors.
3. Count the number of unique roots, which corresponds to the number of islands.

```javascript
class UnionFind {
    constructor(size) {
        this.parent = Array.from({ length: size }, (_, i) => i);
        this.rank = Array(size).fill(1);
        this.count = 0; // To track number of unique sets (islands)
    }

    find(x) {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]); // Path compression
        }
        return this.parent[x];
    }

    union(x, y) {
        let rootX = this.find(x);
        let rootY = this.find(y);

        if (rootX !== rootY) {
            // Union by rank
            if (this.rank[rootX] > this.rank[rootY]) {
                this.parent[rootY] = rootX;
            } else if (this.rank[rootX] < this.rank[rootY]) {
                this.parent[rootX] = rootY;
            } else {
                this.parent[rootY] = rootX;
                this.rank[rootX] += 1;
            }
            this.count--;
        }
    }

    setCount(count) {
        this.count = count;
    }

    getCount() {
        return this.count;
    }
}

var numIslands = function(grid) {
    if (!grid || grid.length === 0) return 0;

    const numRows = grid.length;
    const numCols = grid[0].length;

    const totalCells = numRows * numCols;
    const uf = new UnionFind(totalCells);

    let islandCount = 0;

    for (let i = 0; i < numRows; i++) {
        for (let j = 0; j < numCols; j++) {
            if (grid[i][j] === '1') {
                uf.setCount(uf.getCount() + 1);
                grid[i][j] = '0';
                if (i > 0 && grid[i - 1][j] === '1') { 
                    uf.union(i * numCols + j, (i - 1) * numCols + j);
                }
                if (i < numRows - 1 && grid[i + 1][j] === '1') { 
                    uf.union(i * numCols + j, (i + 1) * numCols + j);
                }
                if (j > 0 && grid[i][j - 1] === '1') { 
                    uf.union(i * numCols + j, i * numCols + j - 1);
                }
                if (j < numCols - 1 && grid[i][j + 1] === '1') { 
                    uf.union(i * numCols + j, i * numCols + j + 1);
                }
            }
        }
    }

    return uf.getCount();
};
```

#### Complexity Analysis
- **Time Complexity**: O(M * N * α(M * N)) where M is the number of rows, N is the number of columns, and α is the Inverse Ackermann function that grows very slowly, making it near constant.
- **Space Complexity**: O(M * N) to store the parent and rank for each cell.

Each of these approaches effectively analyzes the problem using different graph traversal and connected component discovery techniques, suitable for different constraints and efficiency requirements.

