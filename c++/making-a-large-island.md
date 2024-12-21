# [LeetCode 827: Making A Large Island](https://leetcode.com/problems/making-a-large-island/)

## Approaches:

- [Approach 1: Brute Force with DFS](#approach-1-brute-force-with-dfs)
- [Approach 2: Union-Find with Optimization](#approach-2-union-find-with-optimization)

### Approach 1: Brute Force with DFS

**Intuition:**

The basic idea is to examine each 0 and try turning it into a 1, followed by calculating the size of the connected components formed. We do this by using Depth-First Search (DFS) to find the size of islands in the grid.

1. For each cell, if it is part of the water (0), switch it to land (1).
2. Perform a DFS from this cell to compute the area of the new island.
3. Track the maximum island size formed in this manner.
4. Restore the cell to its original state after processing.

**Steps:**

1. Iterate over the grid.
2. For every cell that is 0, change it to 1 temporarily.
3. Use DFS to compute the island size from this cell.
4. Compare sizes to determine the maximum.
5. Restore the grid to its initial state after computation for each cell.

**Code:**

```cpp
class Solution {
public:
    int n;
    
    // Perform DFS to compute the area of an island
    int dfs(vector<vector<int>>& grid, int x, int y) {
        if (x < 0 || x >= n || y < 0 || y >= n || grid[x][y] != 1) {
            return 0;
        }
        grid[x][y] = 2; // Mark as visited
        int area = 1;
        // Explore all 4 directions
        area += dfs(grid, x + 1, y);
        area += dfs(grid, x - 1, y);
        area += dfs(grid, x, y + 1);
        area += dfs(grid, x, y - 1);
        return area;
    }
    
    int largestIsland(vector<vector<int>>& grid) {
        n = grid.size();
        int maxArea = 0;
        
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) {
                    // Temporarily change to 1
                    grid[i][j] = 1;
                    maxArea = max(maxArea, dfs(grid, i, j));
                    // Restore to 0
                    grid[i][j] = 0;
                }
            }
        }
        
        // Check if the grid is all ones already
        if (maxArea == 0) return n * n;
        
        return maxArea;
    }
};
```

**Time Complexity:** O(n^4), where n is the side length of the grid.

**Space Complexity:** O(n^2), due to recursive call stack and grid modifications.

### Approach 2: Union-Find with Optimization

**Intuition:**

Instead of recalculating the size after transforming each cell, pre-calculate the sizes of all islands using a union-find data structure. For each water cell, calculate the potential size of the island formed by considering joining all surrounding islands.

**Steps:**

1. Use union-find to determine all island sizes as initial components.
2. For each water cell, calculate potential island size it could form by checking its neighboring land cells.
3. Track the largest island size obtained.

**Code:**

```cpp
class Solution {
public:
    int n;
    
    int find(vector<int>& parent, int i) {
        if (parent[i] == i) return i;
        return parent[i] = find(parent, parent[i]);
    }
    
    void unionSet(vector<int>& parent, vector<int>& rank, vector<int>& area, int x, int y) {
        int rootX = find(parent, x), rootY = find(parent, y);
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
                area[rootX] += area[rootY];
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
                area[rootY] += area[rootX];
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
                area[rootX] += area[rootY];
            }
        }
    }
    
    int largestIsland(vector<vector<int>>& grid) {
        n = grid.size();
        vector<int> parent(n * n), rank(n * n, 0), area(n * n, 0);
        
        // Initialize the union-find structure
        for (int i = 0; i < n * n; ++i) {
            parent[i] = i;
        }
        
        // Establish islands and calculate their areas
        int directions[5] = {-1, 0, 1, 0, -1};
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    int index = i * n + j;
                    area[index] = 1;
                    for (int d = 0; d < 4; ++d) {
                        int ni = i + directions[d], nj = j + directions[d + 1];
                        if (ni >= 0 && ni < n && nj >= 0 && nj < n && grid[ni][nj] == 1) {
                            unionSet(parent, rank, area, index, ni * n + nj);
                        }
                    }
                }
            }
        }
        
        int maxArea = 0;
        
        // Calculate current max island size
        for (int i = 0; i < n * n; ++i) {
            if (parent[i] == i) {
                maxArea = max(maxArea, area[i]);
            }
        }
        
        // Evaluate changing each zero
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) {
                    unordered_set<int> seenRoot;
                    int potentialArea = 1;
                    for (int d = 0; d < 4; ++d) {
                        int ni = i + directions[d], nj = j + directions[d + 1];
                        if (ni >= 0 && ni < n && nj >= 0 && nj < n && grid[ni][nj] == 1) {
                            int root = find(parent, ni * n + nj);
                            if (seenRoot.insert(root).second) {
                                potentialArea += area[root];
                            }
                        }
                    }
                    maxArea = max(maxArea, potentialArea);
                }
            }
        }
        
        return maxArea;
    }
};
```

**Time Complexity:** O(n^2), where n is the side length of the grid, particularly efficient due to path compression and rank union.

**Space Complexity:** O(n^2), due to union-find structures and additional space for storing island areas.

