## [Leetcode 827: Making A Large Island](https://leetcode.com/problems/making-a-large-island/)

### Approaches:
- [Approach 1: Brute Force with DFS](#approach-1-brute-force-with-dfs)
- [Approach 2: Union-Find with Optimized DFS](#approach-2-union-find-with-optimized-dfs)

---

### Approach 1: Brute Force with DFS

#### Intuition:
In this problem, we aim to find the largest island that can be created by changing exactly one `0` to `1`. Start by considering each water (0) cell and performing Depth First Search (DFS) to compute the size of the connected component (island) if that zero were converted to one. This brute force approach involves examining every `0`, turning it into `1`, and running DFS to measure the resultant island size.

#### Steps:

1. Start with iterating over each cell in the grid. For each `0`, try changing it to `1`.
2. Use DFS to explore the entire island resulting from the change. Record the size of this island.
3. Track the maximum size obtained through any such modification.
4. Consider only one modification at a time - reset before next iteration.

#### Time and Space Complexity:
- **Time Complexity**: O(n^4). Checking each cell O(n^2) and for each 0 does a DFS O(n^2).
- **Space Complexity**: O(n^2) for the recursive stack space.

```csharp
public int LargestIsland(int[][] grid) {
    int n = grid.Length;
    int maxSize = 0;

    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (grid[i][j] == 0) {
                // Change 0 to 1 and perform a DFS
                grid[i][j] = 1;
                maxSize = Math.Max(maxSize, DFS(grid, i, j, new bool[n][].Select(x => new bool[n]).ToArray()));
                // Restore original grid state
                grid[i][j] = 0;
            }
        }
    }

    // Consider the current largest island without any change
    if (maxSize == 0) return n * n; 

    return maxSize;
}

private int DFS(int[][] grid, int r, int c, bool[][] visited) {
    int n = grid.Length;
    if (r < 0 || c < 0 || r >= n || c >= n || grid[r][c] == 0 || visited[r][c]) return 0;
    visited[r][c] = true;
    return 1 + DFS(grid, r+1, c, visited) + DFS(grid, r-1, c, visited) +
               DFS(grid, r, c+1, visited) + DFS(grid, r, c-1, visited);
}
```

---

### Approach 2: Union-Find with Optimized DFS

#### Intuition:
Using a Union-Find data structure to keep track of connected components (islands) in the grid can efficiently manage this operation. Precompute the size of each component using DFS and store the result. Then, for each `0`, compute the potential island size by considering the union of adjacent components.

#### Steps:

1. Traverse the grid, and for each unvisited land (1), use DFS to calculate the size of the island and assign an island ID.
2. Store the size of each island in a dictionary with its ID as the key.
3. Iterate over each `0` cell, and for each, attempt to connect it to adjacently different islands to form a larger island.
4. Track the greatest island size formed.

#### Time and Space Complexity:
- **Time Complexity**: O(n^2) for DFS and calculating potential maximum island size.
- **Space Complexity**: O(n^2) for storing visited states and island sizes.

```csharp
public int LargestIsland(int[][] grid) {
    int n = grid.Length;
    int currentIslandId = 2;
    Dictionary<int, int> islandSizes = new Dictionary<int, int>(); // island ID to size
    int maxSize = 0;

    // First pass: Assign island ids and save their sizes
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (grid[i][j] == 1) {
                int size = DFSMarkIsland(grid, i, j, currentIslandId);
                islandSizes[currentIslandId] = size;
                maxSize = Math.Max(maxSize, size);
                currentIslandId++;
            }
        }
    }

    // Second pass: Evaluate potential maximum island size by toggling each 0
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (grid[i][j] == 0) {
                HashSet<int> adjacentIslands = new HashSet<int>();
                int potentialSize = 1;
                if (i > 0 && grid[i-1][j] > 1) adjacentIslands.Add(grid[i-1][j]);
                if (i < n-1 && grid[i+1][j] > 1) adjacentIslands.Add(grid[i+1][j]);
                if (j > 0 && grid[i][j-1] > 1) adjacentIslands.Add(grid[i][j-1]);
                if (j < n-1 && grid[i][j+1] > 1) adjacentIslands.Add(grid[i][j+1]);

                foreach (var id in adjacentIslands) {
                    potentialSize += islandSizes[id];
                }

                maxSize = Math.Max(maxSize, potentialSize);
            }
        }
    }
    
    return maxSize;
}

private int DFSMarkIsland(int[][] grid, int r, int c, int id) {
    int n = grid.Length;
    if (r < 0 || c < 0 || r >= n || c >= n || grid[r][c] != 1) return 0;

    grid[r][c] = id;
    return 1 + DFSMarkIsland(grid, r+1, c, id) + DFSMarkIsland(grid, r-1, c, id) +
               DFSMarkIsland(grid, r, c+1, id) + DFSMarkIsland(grid, r, c-1, id);
}
```

This approach is much more optimized and results in linear time complexity for dense grids, making it suitable for large input sizes. By precomputing and storing island sizes, we efficiently calculate potential island expansions using existing union-find ideology without explicitly maintaining it, thus merging components dynamically during exploration.

