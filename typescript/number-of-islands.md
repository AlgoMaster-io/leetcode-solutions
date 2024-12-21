## [Leetcode 200: Number of Islands](https://leetcode.com/problems/number-of-islands/)

### Approaches:
1. [DFS (Depth First Search) Approach](#dfs-approach)
2. [BFS (Breadth First Search) Approach](#bfs-approach)
3. [Union-Find (Disjoint Set) Approach](#union-find-approach)

### DFS Approach

The Depth First Search (DFS) approach entails visiting every component of the grid. We treat the grid as a graph where each '1' (land) is a node and we explore all connected lands using DFS.

#### Intuition:
- Iterate through every cell in the grid. 
- Start from any land '1', and perform a DFS to mark all connected lands.
- Each DFS marks an entire island, so increment the island counter.

#### Time and Space Complexity:
- Time Complexity: O(M * N), where M and N are the dimensions of the grid, because we potentially visit every cell.
- Space Complexity: O(M * N) in the case of a highly skewed recursion stack (worst case).

```typescript
function numIslands(grid: string[][]): number {
    if (!grid || grid.length === 0) return 0;
    let numIslands = 0;

    const dfs = (i: number, j: number) => {
        // Check for boundaries and if it's water or already visited
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[i].length || grid[i][j] === '0') {
            return;
        }

        // Mark the cell as visited by turning '1' to '0'
        grid[i][j] = '0';

        // Traverse all four directions
        dfs(i + 1, j);
        dfs(i - 1, j);
        dfs(i, j + 1);
        dfs(i, j - 1);
    };

    for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
            if (grid[i][j] === '1') {
                numIslands++;
                dfs(i, j); // Start DFS to mark the whole island
            }
        }
    }

    return numIslands;
}
```

### BFS Approach

Similar to DFS, but instead uses a Breadth First Search (BFS) approach, which can be more efficient in some scenarios due to iterative structure.

#### Intuition:
- Iterate over each cell, perform a BFS starting from any land '1'.
- Use a queue to track nodes to be visited in BFS manner.
- Increment island count for each BFS initiated.

#### Time and Space Complexity:
- Time Complexity: O(M * N), same reason as DFS.
- Space Complexity: O(min(M, N)) due to the queue holding layer nodes at one time.

```typescript
function numIslands(grid: string[][]): number {
    if (!grid || grid.length === 0) return 0;
    let numIslands = 0;
    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];

    const bfs = (i: number, j: number) => {
        const queue: [number, number][] = [];
        queue.push([i, j]);
        grid[i][j] = '0';

        while (queue.length > 0) {
            const [x, y] = queue.shift()!;

            for (const [dx, dy] of directions) {
                const newX = x + dx;
                const newY = y + dy;
                if (newX >= 0 && newX < grid.length && newY >= 0 && newY < grid[newX].length && grid[newX][newY] === '1') {
                    grid[newX][newY] = '0';
                    queue.push([newX, newY]);
                }
            }
        }
    };

    for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
            if (grid[i][j] === '1') {
                numIslands++;
                bfs(i, j); // Perform BFS to mark the island.
            }
        }
    }

    return numIslands;
}
```

### Union-Find Approach

Uses a disjoint-set data structure to dynamically connect nodes into sets representing islands. This method is highly efficient for recognizing connected components.

#### Intuition:
- Treat each '1' as a separate potential component.
- Iterate over cells; union adjacent '1's left and above, reducing component count.
- Components represent islands, tracked using union-find operations.

#### Time and Space Complexity:
- Time Complexity: O(M * N * α(M * N)), where α is the Inverse Ackermann function which is nearly constant for practical sizes.
- Space Complexity: O(M * N) for the union-find structures.

```typescript
class UnionFind {
    parent: number[];
    rank: number[];
    count: number;

    constructor(size: number) {
        this.parent = Array(size).fill(0).map((_, index) => index);
        this.rank = Array(size).fill(1);
        this.count = 0;
    }

    find(x: number): number {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]);
        }
        return this.parent[x];
    }

    union(x: number, y: number) {
        const rootX = this.find(x);
        const rootY = this.find(y);

        if (rootX !== rootY) {
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

    setCount(count: number) {
        this.count = count;
    }

    getCount() {
        return this.count;
    }
}

function numIslands(grid: string[][]): number {
    if (!grid || grid.length === 0) return 0;

    const m = grid.length;
    const n = grid[0].length;
    const uf = new UnionFind(m * n);

    let count = 0;
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === '1') {
                count++;
            }
        }
    }

    uf.setCount(count);

    const directions = [[1, 0], [0, 1]];  // Only check right and down to avoid double counting
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === '1') {
                for (const [dx, dy] of directions) {
                    const x = i + dx;
                    const y = j + dy;
                    if (x < m && y < n && grid[x][y] === '1') {
                        uf.union(i * n + j, x * n + y);
                    }
                }
            }
        }
    }

    return uf.getCount();
}
```

Each solution demonstrated different techniques with increasing efficiency, precisely tackling the problem suited to specific constraints.

