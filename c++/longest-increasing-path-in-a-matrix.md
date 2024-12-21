# [Leetcode 329: Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Table of Contents
1. [Naive DFS Approach](#naive-dfs-approach)
2. [DFS with Memoization](#dfs-with-memoization)
3. [Topological Sort via Kahn’s Algorithm](#topological-sort-via-kahns-algorithm)

---

### Naive DFS Approach

#### Intuition
The simplest way to approach this problem is by using Depth First Search (DFS) from every cell. For each cell, perform a DFS to find the longest increasing path starting from that cell by exploring its neighbors. We choose to go to a neighbor if it's greater than the current cell value. Since we would explore every possible path multiple times, this approach is inefficient.

#### Code

```cpp
class Solution {
public:
    int directions[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    
    int dfs(vector<vector<int>>& matrix, int i, int j) {
        int maxPath = 1;  // The path includes itself
        for (auto& dir : directions) {
            int x = i + dir[0], y = j + dir[1];
            if (x >= 0 && x < matrix.size() && y >= 0 && y < matrix[0].size() && matrix[x][y] > matrix[i][j]) {
                // Perform DFS and find the maximum increasing path from neighbor
                maxPath = max(maxPath, 1 + dfs(matrix, x, y));
            }
        }
        return maxPath;
    }
    
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.empty()) return 0;
        int maxPath = 0;
        for (int i = 0; i < matrix.size(); ++i) {
            for (int j = 0; j < matrix[0].size(); ++j) {
                // Compute the longest path starting from each cell
                maxPath = max(maxPath, dfs(matrix, i, j));
            }
        }
        return maxPath;
    }
};
```

#### Time Complexity
- **O((m * n) * 4^(m*n))**, where \( m \) is the number of rows and \( n \) is the number of columns, as DFS runs until depth equals the number of cells.

#### Space Complexity
- **O(m * n)**, due to recursion stack.

---

### DFS with Memoization

#### Intuition
We can optimize the naive DFS approach by memoizing the results of longest paths starting from each cell to avoid redundant calculations. If a path from a cell has already been computed before, just return that value instead of recalculating.

#### Code

```cpp
class Solution {
public:
    int directions[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    
    int dfs(vector<vector<int>>& matrix, int i, int j, vector<vector<int>>& memo) {
        if (memo[i][j] != 0) return memo[i][j];  // Return cached result if available
        
        int maxPath = 1;
        for (auto& dir : directions) {
            int x = i + dir[0], y = j + dir[1];
            if (x >= 0 && x < matrix.size() && y >= 0 && y < matrix[0].size() && matrix[x][y] > matrix[i][j]) {
                maxPath = max(maxPath, 1 + dfs(matrix, x, y, memo));
            }
        }
        return memo[i][j] = maxPath;  // Cache result
    }
    
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.empty()) return 0;
        int maxPath = 0;
        vector<vector<int>> memo(matrix.size(), vector<int>(matrix[0].size(), 0));
        
        for (int i = 0; i < matrix.size(); ++i) {
            for (int j = 0; j < matrix[0].size(); ++j) {
                maxPath = max(maxPath, dfs(matrix, i, j, memo));
            }
        }
        return maxPath;
    }
};
```

#### Time Complexity
- **O(m * n)**, since each cell's longest path is computed only once and results are reused.

#### Space Complexity
- **O(m * n)**, for memoization table and recursion stack.

---

### Topological Sort via Kahn’s Algorithm

#### Intuition
Another optimal approach is to think of this matrix as a directed graph and use Topological Sort. Cells point to neighbors with greater numbers. We can then follow the notion of processing cells in increasing order by using Kahn’s algorithm for topological sorting with a queue.

#### Code

```cpp
class Solution {
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.empty()) return 0;
        
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> indegree(m, vector<int>(n, 0));
        queue<pair<int, int>> q;
        
        int directions[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (auto& dir : directions) {
                    int x = i + dir[0], y = j + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[i][j] < matrix[x][y]) {
                        indegree[x][y]++;
                    }
                }
            }
        }
        
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (indegree[i][j] == 0)
                    q.push({i, j});
            }
        }
        
        int length = 0;
        while (!q.empty()) {
            int size = q.size();
            length++;
            for (int k = 0; k < size; ++k) {
                auto [i, j] = q.front(); q.pop();
                for (auto& dir : directions) {
                    int x = i + dir[0], y = j + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[i][j] < matrix[x][y]) {
                        indegree[x][y]--;
                        if (indegree[x][y] == 0) {
                            q.push({x, y});
                        }
                    }
                }
            }
        }
        
        return length;
    }
};
```

#### Time Complexity
- **O(m * n)**, each cell is enqueued and processed once.

#### Space Complexity
- **O(m * n)**, for storing indegree of each node.

