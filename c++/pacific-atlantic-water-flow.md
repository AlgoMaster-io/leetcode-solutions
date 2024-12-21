# [Leetcode 417: Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)

**Approaches**

1. [Basic DFS Approach](#approach-1-basic-dfs-approach)
2. [Optimized Two-BFS Approach](#approach-2-optimized-two-bfs-approach)

## Approach 1: Basic DFS Approach

### Intuition

The problem essentially asks us to find cells from which water can flow to both the Pacific and Atlantic Oceans. The cells reachable by the Pacific ocean border on the top or left edges, and those reachable by the Atlantic border on the bottom or right edges.

A straightforward approach is to perform a depth-first search (DFS) from all cells adjacent to each ocean and mark the reachable cells. The intersection of the two results will give the desired cells.

### Steps

1. Use a matrix `pacific` to mark cells from which water can flow to the Pacific Ocean.
2. Use another matrix `atlantic` for cells flowing to the Atlantic.
3. Perform a DFS from all cells on the left and top (for Pacific) and the right and bottom (for Atlantic).
4. For each cell, recursively check in all four directions if the height condition (neighbor height is less than or equal to current) allows water flow.
5. The intersection of the `true` entries in both matrices will give the result.

### Code

```cpp
class Solution {
public:
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        vector<vector<bool>> pacific(m, vector<bool>(n, false));
        vector<vector<bool>> atlantic(m, vector<bool>(n, false));
        vector<pair<int, int>> dir = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        
        // Helper function for DFS
        function<void(int, int, vector<vector<bool>> &ocean)> dfs = [&](int r, int c, vector<vector<bool>> &ocean) {
            ocean[r][c] = true;
            for (auto [dr, dc] : dir) {
                int nr = r + dr, nc = c + dc;
                // Ensure the new position is inside the grid and the new position is not yet visited
                if (nr >= 0 && nr < m && nc >= 0 && nc < n && !ocean[nr][nc] && heights[nr][nc] >= heights[r][c]) {
                    dfs(nr, nc, ocean);
                }
            }
        };
        
        // Run DFS for each ocean border
        for (int i = 0; i < m; ++i) {
            dfs(i, 0, pacific); // left edge
            dfs(i, n - 1, atlantic); // right edge
        }
        for (int j = 0; j < n; ++j) {
            dfs(0, j, pacific); // top edge
            dfs(m - 1, j, atlantic); // bottom edge
        }
        
        // Collect intersection cells
        vector<vector<int>> result;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (pacific[i][j] && atlantic[i][j]) {
                    result.push_back({i, j});
                }
            }
        }
        
        return result;
    }
};
```

### Time Complexity

- The DFS is performed at most once for each cell. Therefore, the time complexity is \(O(m \times n)\), where \(m\) is the number of rows and \(n\) is the number of columns in the grid.

### Space Complexity

- Space complexity is \(O(m \times n)\) due to the additional grids required for `pacific` and `atlantic` and the recursion stack in the worst case.

## Approach 2: Optimized Two-BFS Approach

### Intuition

We can optimize our search by starting BFS from all cells adjacent to each ocean simultaneously. This allows both oceans’ BFS to run concurrently until they cover the entire grid. In comparison to DFS, BFS is advantageous in terms of iterative control.

### Steps

1. Utilize a queue to implement BFS and initiate the BFS from all border cells that can reach the Pacific.
2. Similarly, perform BFS starting from all border cells for the Atlantic.
3. During BFS, enqueue cells whose neighboring cells have a height condition satisfied.
4. The intersection of visited cells will yield the result.

### Code

```cpp
class Solution {
public:
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        vector<vector<bool>> pacific(m, vector<bool>(n, false));
        vector<vector<bool>> atlantic(m, vector<bool>(n, false));
        queue<pair<int, int>> pq, aq;
        vector<pair<int, int>> dir = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

        // Enqueue initial cells along the oceans
        for (int i = 0; i < m; ++i) {
            pq.push({i, 0});
            aq.push({i, n - 1});
            pacific[i][0] = true;
            atlantic[i][n - 1] = true;
        }
        for (int j = 0; j < n; ++j) {
            pq.push({0, j});
            aq.push({m - 1, j});
            pacific[0][j] = true;
            atlantic[m - 1][j] = true;
        }

        // BFS function
        auto bfs = [&](queue<pair<int, int>>& q, vector<vector<bool>>& ocean){
            while (!q.empty()) {
                auto [r, c] = q.front();
                q.pop();
                for (auto [dr, dc] : dir) {
                    int nr = r + dr, nc = c + dc;
                    if (nr >= 0 && nr < m && nc >= 0 && nc < n && !ocean[nr][nc] && heights[nr][nc] >= heights[r][c]) {
                        ocean[nr][nc] = true;
                        q.push({nr, nc});
                    }
                }
            }
        };

        bfs(pq, pacific);
        bfs(aq, atlantic);

        // Intersection of both matrices
        vector<vector<int>> result;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (pacific[i][j] && atlantic[i][j]) {
                    result.push_back({i, j});
                }
            }
        }
        
        return result;
    }
};
```

### Time Complexity

- Similar to the DFS approach, this method also requires \(O(m \times n)\) time complexity because each cell is enqueued and dequeued at most once.

### Space Complexity

- Space complexity remains \(O(m \times n)\) due to the space needed for the ocean matrices and the queues used for BFS.

