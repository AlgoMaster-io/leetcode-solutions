# [Leetcode 827: Making A Large Island](https://leetcode.com/problems/making-a-large-island/)

## Approaches:
- [Naive Approach](#naive-approach)
- [Union-Find with DFS](#union-find-with-dfs)

### Naive Approach

#### Intuition:
A straightforward approach to this problem is to identify connected components of land (1s) and calculate their sizes. For each possible water cell (0), we temporarily convert it into a land cell and compute the potential largest island size by merging adjacent components.

#### Steps:
1. Iterate over the grid and apply DFS or BFS to identify all islands and compute their areas.
2. For each water (0) cell:
   - Temporarily change it to land (1).
   - Calculate the possible largest island size by considering connected components.
   - Restore the cell back to water (0).
3. Return the maximum island size found during these conversions.

#### Example Code:

```javascript
function largestIsland(grid) {
    const n = grid.length;
    
    // Helper function to perform DFS and calculate island area
    const dfs = (i, j, label, areas) => {
        if (i < 0 || i >= n || j < 0 || j >= n || grid[i][j] !== 1) {
            return 0;
        }
        
        grid[i][j] = label;  // Label the island
        let area = 1;  // Start with area of 1 for this cell

        // Explore all 4 directions
        area += dfs(i + 1, j, label, areas);
        area += dfs(i - 1, j, label, areas);
        area += dfs(i, j + 1, label, areas);
        area += dfs(i, j - 1, label, areas);
        
        return area;
    };
    
    // Label each island with a unique label and calculate areas
    let label = 2;
    const areas = {};
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 1) {
                const area = dfs(i, j, label, areas);
                areas[label] = area;
                label++;
            }
        }
    }
    
    // Return area of the largest island after converting one 0
    let maxArea = Math.max(0, ...Object.values(areas));

    // Try converting each 0 to a 1 and calculate the potential island size
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 0) {
                const possibleIslands = new Set();
                const deltas = [[1, 0], [-1, 0], [0, 1], [0, -1]];
                for (const [dx, dy] of deltas) {
                    const ni = i + dx, nj = j + dy;
                    if (ni >= 0 && ni < n && nj >= 0 && nj < n && grid[ni][nj] > 1) {
                        possibleIslands.add(grid[ni][nj]);
                    }
                }
                let potentialArea = 1;  // Initial conversion area
                for (const lbl of possibleIslands) {
                    potentialArea += areas[lbl];
                }
                maxArea = Math.max(maxArea, potentialArea);
            }
        }
    }

    return maxArea;
}
```

#### Complexity:
- **Time Complexity:** \(O(N^2)\), where N is the dimension of the grid. We traverse the entire grid multiple times.
- **Space Complexity:** \(O(N^2)\) in the worst case, due to space used in the recursive stack of DFS.

---

### Union-Find with DFS

#### Intuition:
A more optimal approach uses a union-find data structure to manage connected components and their sizes. This allows for efficient merging of islands when converting a water cell, leveraging path compression and union by rank for efficient operations.

#### Steps:
1. Use DFS to find the initial islands and assign a unique component ID to each.
2. Use union-find to track and manage the sizes of these connected components.
3. For each zero cell, calculate potential new island sizes by considering adjacent unique components.

#### Code Example:

```javascript
function largestIsland(grid) {
    const n = grid.length;
    const parent = new Array(n * n);
    const size = new Array(n * n).fill(1);
    
    // Find function with path compression
    const find = (i) => {
        if (parent[i] !== i) {
            parent[i] = find(parent[i]);
        }
        return parent[i];
    };
    
    // Union function with union by size
    const union = (i, j) => {
        const rootI = find(i);
        const rootJ = find(j);
        if (rootI !== rootJ) {
            if (size[rootI] > size[rootJ]) {
                parent[rootJ] = rootI;
                size[rootI] += size[rootJ];
            } else {
                parent[rootI] = rootJ;
                size[rootJ] += size[rootI];
            }
        }
    };
    
    // Initialize parents
    for (let i = 0; i < n*n; i++) {
        parent[i] = i;
    }
    
    // Convert 2D grid position to a single index
    const getIndex = (i, j) => i * n + j;

    // Union all 1s that are adjacent
    const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 1) {
                for (const [dx, dy] of directions) {
                    const ni = i + dx, nj = j + dy;
                    if (ni >= 0 && nj >= 0 && ni < n && nj < n && grid[ni][nj] === 1) {
                        union(getIndex(i, j), getIndex(ni, nj));
                    }
                }
            }
        }
    }
    
    // Find the largest island by converting one 0 to 1
    let maxArea = 0;
    const seenComponents = new Set();
    
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 0) {
                let potentialArea = 1;
                seenComponents.clear();
                
                for (const [dx, dy] of directions) {
                    const ni = i + dx, nj = j + dy;
                    if (ni >= 0 && nj >= 0 && ni < n && nj < n && grid[ni][nj] === 1) {
                        const rootId = find(getIndex(ni, nj)); // find root id
                        if (!seenComponents.has(rootId)) {
                            potentialArea += size[rootId];
                            seenComponents.add(rootId);
                        }
                    }
                }
                maxArea = Math.max(maxArea, potentialArea);
            }
        }
    }

    // If no 0 was converted, that means there are no 0s, return n*n
    return maxArea === 0 ? n * n : maxArea;
}
```

#### Complexity:
- **Time Complexity:** \(O(N^2 \alpha(N^2))\), where \(\alpha\) is the inverse Ackermann function, representing amortized time complexity for union-find operations.
- **Space Complexity:** \(O(N^2)\), for storing parent and size arrays.

Each solution progressively improves the efficiency of calculating the largest possible island size by effectively managing connected components. The union-find approach leverages efficient merging and find operations to handle larger grids more optimally.

