# [Leetcode 827: Making A Large Island](https://leetcode.com/problems/making-a-large-island/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Union-Find Approach](#union-find-approach)

---

### Brute Force Approach

#### Intuition:
The brute force approach involves checking each zero in the grid and temporarily flipping it to one. After that, flood-fill the grid to find the size of this connected component and store the maximum size encountered. This involves revisiting each cell individually and recalculating sizes for each zero, which is computationally heavy.

#### Implementation:

```typescript
function largestIsland(grid: number[][]): number {
    const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
    const n = grid.length;

    function dfs(r: number, c: number, visited: Set<string>): number {
        visited.add(`${r},${c}`);
        let size = 1;
        for (const [dr, dc] of directions) {
            const nr = r + dr;
            const nc = c + dc;
            if (nr >= 0 && nr < n && nc >= 0 && nc < n && grid[nr][nc] == 1 && !visited.has(`${nr},${nc}`)) {
                size += dfs(nr, nc, visited);
            }
        }
        return size;
    }

    let maxIslandSize = 0;

    // Check the largest island without flipping any zero
    const visitedInitial = new Set<string>();
    for (let r = 0; r < n; r++) {
        for (let c = 0; c < n; c++) {
            if (grid[r][c] == 1 && !visitedInitial.has(`${r},${c}`)) {
                const size = dfs(r, c, visitedInitial);
                maxIslandSize = Math.max(maxIslandSize, size);
            }
        }
    }
  
    // If the grid is already one big island
    if (maxIslandSize === n * n) return maxIslandSize;

    // Try converting each 0 to 1 and calculate possible new island size
    for (let r = 0; r < n; r++) {
        for (let c = 0; c < n; c++) {
            if (grid[r][c] == 0) {
                grid[r][c] = 1;
                const visited = new Set<string>();
                const size = dfs(r, c, visited);
                maxIslandSize = Math.max(maxIslandSize, size);
                grid[r][c] = 0; // flip back
            }
        }
    }

    return maxIslandSize;
}
```

#### Complexity Analysis:
- **Time Complexity:** O(N^4), where N is the side length of the grid. Each 0 has an additional depth-first search of O(N^2).
- **Space Complexity:** O(N^2), due to the space required for the visited set.

---

### Union-Find Approach

#### Intuition:
This method optimizes the brute force approach by using Union-Find (also known as Disjoint Set Union, DSU) to efficiently manage connected components. Each component is assigned an ID, and we compute the size once when we traverse the grid initially. Then, for each zero, we calculate a potential island size as the sum of sizes of all unique neighboring components plus one, without needing to recompute each time.

#### Implementation:

```typescript
function largestIsland(grid: number[][]): number {
    const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
    const n = grid.length;

    const parent: number[] = [];
    const size: number[] = [];
    let id = 2; // Start IDs from 2 since 0 and 1 are used in the grid

    function find(x: number): number {
        if (parent[x] === x) return x;
        return parent[x] = find(parent[x]);
    }

    function union(x: number, y: number): void {
        const rootX = find(x);
        const rootY = find(y);
        if (rootX !== rootY) {
            if (size[rootX] > size[rootY]) {
                parent[rootY] = rootX;
                size[rootX] += size[rootY];
            } else {
                parent[rootX] = rootY;
                size[rootY] += size[rootX];
            }
        }
    }

    // Initialize parent and size arrays
    for (let i = 0; i < n * n + 2; i++) {
        parent[i] = i;
        size[i] = 1;
    }

    // Assign component IDs and calculate initial sizes
    for (let r = 0; r < n; r++) {
        for (let c = 0; c < n; c++) {
            if (grid[r][c] == 1) {
                grid[r][c] = id;
                for (const [dr, dc] of directions) {
                    const nr = r + dr;
                    const nc = c + dc;
                    if (nr >= 0 && nr < n && nc >= 0 && nc < n && grid[nr][nc] > 1) {
                        union(r * n + c, nr * n + nc);
                    }
                }
                id++;
            }
        }
    }

    let maxIslandSize = 0;

    // Find max island size without any conversion
    for (let r = 0; r < n; r++) {
        for (let c = 0; c < n; c++) {
            if (grid[r][c] > 1) {
                maxIslandSize = Math.max(maxIslandSize, size[find(r * n + c)]);
            }
        }
    }

    // Try each 0 to become 1 and check size
    for (let r = 0; r < n; r++) {
        for (let c = 0; c < n; c++) {
            if (grid[r][c] == 0) {
                let newSize = 1; // Start with one for the new cell
                const seen = new Set<number>();
                for (const [dr, dc] of directions) {
                    const nr = r + dr;
                    const nc = c + dc;
                    if (nr >= 0 && nr < n && nc >= 0 && nc < n && grid[nr][nc] > 1) {
                        const rootId = find(nr * n + nc);
                        if (!seen.has(rootId)) {
                            seen.add(rootId);
                            newSize += size[rootId];
                        }
                    }
                }
                maxIslandSize = Math.max(maxIslandSize, newSize);
            }
        }
    }

    return maxIslandSize;
}
```

#### Complexity Analysis:
- **Time Complexity:** O(N^2), for traversing the grid and union-find operations which are nearly constant due to path compression.
- **Space Complexity:** O(N^2), for storing the parent and size arrays.
  
This approach significantly reduces redundant computation by leveraging efficient union-find data structures to manage connected components.

