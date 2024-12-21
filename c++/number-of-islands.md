# [Leetcode 200: Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Table of Contents
- [Approach 1: DFS (Depth-First Search) Approach](#approach-1-dfs-depth-first-search-approach)
- [Approach 2: BFS (Breadth-First Search) Approach](#approach-2-bfs-breadth-first-search-approach)
- [Approach 3: Union-Find Approach](#approach-3-union-find-approach)

---

## Approach 1: DFS (Depth-First Search) Approach

### Intuition

The problem is about counting the number of distinct connected components (islands) in a grid. This can be intuitively described using graph traversal techniques. In the DFS approach:
- We traverse each cell in the grid.
- When a land cell ('1') is found, we initiate a DFS from this cell to mark all connected land cells as visited (by changing them to '0' or any other marker).
- Increment a counter each time a new DFS is initiated, which corresponds to discovering a new island.

### Code

```cpp
class Solution {
public:
    void dfs(vector<vector<char>>& grid, int i, int j) {
        // Check for out-of-bounds or water cell
        if (i < 0 || i >= grid.size() || j < 0 || j >= grid[0].size() || grid[i][j] == '0') {
            return;
        }
        
        // Mark the current cell as visited by changing it to '0'
        grid[i][j] = '0';
        
        // Run DFS in all 4 possible directions (up, down, left, right)
        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i, j - 1);
    }
    
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty()) {
            return 0;
        }
        
        int numberOfIslands = 0;
        
        // Traverse each cell in the grid
        for (int i = 0; i < grid.size(); ++i) {
            for (int j = 0; j < grid[0].size(); ++j) {
                // If a land cell is found
                if (grid[i][j] == '1') {
                    // Perform DFS to mark all connected cells
                    dfs(grid, i, j);
                    // Increment the island counter
                    ++numberOfIslands;
                }
            }
        }
        
        return numberOfIslands; // This is the total number of islands found
    }
};
```

### Time Complexity
- **Time Complexity:** O(M * N), where M is the number of rows and N is the number of columns. We visit each cell once.
- **Space Complexity:** O(M * N) in the worst case due to the recursion stack.

---

## Approach 2: BFS (Breadth-First Search) Approach

### Intuition

Similar to the DFS approach, BFS can also explore the entire island when it encounters land. BFS uses a queue to explore the island in layers:
- Start by pushing the initial land cell onto a queue.
- Visit each cell, marking it as water, and enqueue its adjacent land cells.
- Increment the count of islands when starting a new BFS traversal from an unvisited land cell.

### Code

```cpp
#include <queue>

class Solution {
public:
    void bfs(vector<vector<char>>& grid, int i, int j) {
        queue<pair<int, int>> q;
        q.push({i, j});
        grid[i][j] = '0'; // mark as visited

        // directions array: up, down, left, right
        vector<pair<int, int>> directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        
        while(!q.empty()) {
            auto [x, y] = q.front();
            q.pop();
            for (auto dir : directions) {
                int newX = x + dir.first;
                int newY = y + dir.second;
                if (newX >= 0 && newX < grid.size() && newY >= 0 && newY < grid[0].size() && grid[newX][newY] == '1') {
                    grid[newX][newY] = '0'; // mark as visited
                    q.push({newX, newY});
                }
            }
        }
    }
    
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty()) return 0;
        
        int numberOfIslands = 0;
        
        // Traverse each cell in the grid
        for (int i = 0; i < grid.size(); ++i) {
            for (int j = 0; j < grid[0].size(); ++j) {
                if (grid[i][j] == '1') {
                    bfs(grid, i, j);
                    ++numberOfIslands;
                }
            }
        }
        
        return numberOfIslands; // Total number of islands found
    }
};
```

### Time Complexity
- **Time Complexity:** O(M * N), where M is the number of rows and N is the number of columns. Each cell is visited once.
- **Space Complexity:** O(min(M, N)) for the queue used in BFS.
---

## Approach 3: Union-Find Approach

### Intuition

Union-Find is a data structure effective for dealing with dynamic connectivity queries. Here, it allows us to group land cells and efficiently determine the number of distinct sets (islands). This approach is more complex but elegant:
- Each land cell is initially considered a separate island.
- As we process each cell, we unite it with its adjacent land cells.
- The total number of unique sets (found using the number of roots) represents the number of islands.

### Code

```cpp
class UnionFind {
private:
    vector<int> parent;
    int count;

public:
    UnionFind(int n) : parent(n), count(n) {
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }
    
    int find(int i) {
        if (parent[i] != i) {
            parent[i] = find(parent[i]);
        }
        return parent[i];
    }
    
    void unionSet(int i, int j) {
        int root1 = find(i);
        int root2 = find(j);
        if (root1 != root2) {
            parent[root1] = root2;
            --count; // One less disjoint set after union
        }
    }
    
    int getCount() const {
        return count;
    }
};

class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty() || grid[0].empty()) return 0;

        int m = grid.size();
        int n = grid[0].size();
        int numberOfIslands = 0;
        UnionFind uf(m * n);

        // Initialize count of separate islands
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '1') {
                    ++numberOfIslands;
                }
            }
        }
        
        // Union adjacent lands
        vector<pair<int, int>> directions = {{1, 0}, {0, 1}};
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '1') {
                    for (auto dir : directions) {
                        int newI = i + dir.first;
                        int newJ = j + dir.second;
                        if (newI < m && newJ < n && grid[newI][newJ] == '1') {
                            // Convert 2D grid indices to 1D for Union Find
                            int id1 = i * n + j;
                            int id2 = newI * n + newJ;
                            uf.unionSet(id1, id2);
                        }
                    }
                }
            }
        }

        return uf.getCount() - (m * n - numberOfIslands);
    }
};
```

### Time Complexity
- **Time Complexity:** O(M * N * α(M * N)), where α is the inverse Ackermann function, which grows very slowly.
- **Space Complexity:** O(M * N) due to the union-find data structure.

These methods explore different strategies to solve the same problem, offering a range of complexity and efficiency options tailored to specific requirements like runtime performance and space utilization.

