# [Leetcode 130: Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

In this problem, we are given an m x n matrix board containing 'X' and 'O'. We need to capture all regions surrounded by 'X'. A region is captured by flipping all 'O's into 'X's in that surrounded region.

## Approaches

1. [Depth-First Search (DFS) Approach](#dfs-approach)
2. [Breadth-First Search (BFS) Approach](#bfs-approach)
3. [Union-Find Approach](#union-find-approach)

### DFS Approach

#### Intuition
The idea behind using DFS is that any 'O' connected to the boundary can't be flipped; thus, any 'O' not connected to the boundary needs flipping.
1. Traverse the boundary of the board and for any 'O' found, start a DFS and mark all connected 'O's with a temporary marker (say, 'T').
2. After all DFS runs are complete, flip all remaining 'O's to 'X' and convert all 'T's back to 'O'.

#### Code

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    void solve(vector<vector<char>>& board) {
        if (board.empty()) return;

        int m = board.size();
        int n = board[0].size();
        
        // Helper lambda function for DFS traversal
        auto dfs = [&](int x, int y, auto&& dfs_ref) -> void {
            if (x < 0 || x >= m || y < 0 || y >= n || board[x][y] != 'O') {
                return;
            }
            // Mark the cell as 'T' to indicate it's explored
            board[x][y] = 'T';

            // Explore neighbors
            dfs_ref(x + 1, y, dfs_ref); // downward
            dfs_ref(x - 1, y, dfs_ref); // upward
            dfs_ref(x, y + 1, dfs_ref); // right
            dfs_ref(x, y - 1, dfs_ref); // left
        };

        // Check the boundaries
        for (int i = 0; i < m; ++i) {
            dfs(i, 0, dfs);
            dfs(i, n - 1, dfs);
        }
        for (int j = 0; j < n; ++j) {
            dfs(0, j, dfs);
            dfs(m - 1, j, dfs);
        }

        // Traverse the board again to flip the regions
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                } else if (board[i][j] == 'T') {
                    board[i][j] = 'O';
                }
            }
        }
    }
};
```

#### Time Complexity
- The time complexity of this DFS approach is O(m * n) because in the worst case we need to visit each cell once.

#### Space Complexity
- The space complexity is O(m * n) considering the recursion stack in the worst case when all cells are 'T's.

### BFS Approach

#### Intuition
Similar to DFS but using a queue to iteratively process each 'O' connected to a boundary.

1. Utilize a queue to perform a BFS for each boundary 'O'.
2. Mark cells as 'T'.
3. Finally, flip the inner 'O's to 'X' and revert 'T's to 'O'.

#### Code

```cpp
#include <vector>
#include <queue>
using namespace std;

class Solution {
public:
    void solve(vector<vector<char>>& board) {
        if (board.empty()) return;

        int m = board.size();
        int n = board[0].size();
        queue<pair<int, int>> q;

        auto bfs = [&](int x, int y) {
            q.push({x, y});
            board[x][y] = 'T'; // mark as visited
            while (!q.empty()) {
                auto [cx, cy] = q.front(); q.pop();
                // Check all four directions
                for (auto [nx, ny] : vector<pair<int, int>>{{cx+1, cy}, {cx-1, cy}, {cx, cy+1}, {cx, cy-1}}) {
                    if (nx >= 0 && nx < m && ny >= 0 && ny < n && board[nx][ny] == 'O') {
                        q.push({nx, ny});
                        board[nx][ny] = 'T'; // mark as visited
                    }
                }
            }
        };
        
        // Traverse the boundaries
        for (int i = 0; i < m; ++i) {
            if (board[i][0] == 'O') bfs(i, 0);
            if (board[i][n-1] == 'O') bfs(i, n-1);
        }
        for (int j = 0; j < n; ++j) {
            if (board[0][j] == 'O') bfs(0, j);
            if (board[m-1][j] == 'O') bfs(m-1, j);
        }

        // After BFS, flip the remaining 'O' to 'X' and 'T' back to 'O'
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                } else if (board[i][j] == 'T') {
                    board[i][j] = 'O';
                }
            }
        }
    }
};
```

#### Time Complexity
- Same as DFS, O(m * n), as BFS explores each cell once.

#### Space Complexity
- O(m * n), due to the space usage of the queue potentially holding all cells in the worst case.

### Union-Find Approach

#### Intuition
Using Union-Find, we connect boundary 'O's with a virtual node, ensuring we don't flip any 'O' connected to this virtual node.

1. Treat every 'O' as a node and perform union operations.
2. Connect all border 'O's to a virtual node.
3. Traverse the matrix and flip 'O's not connected to the virtual node.

#### Code

```cpp
#include <vector>
using namespace std;

class UnionFind {
private:
    vector<int> parent;
    vector<int> rank;
public:
    UnionFind(int n) : parent(n), rank(n, 1) {
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // path compression
        }
        return parent[x];
    }
    
    void unionSet(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
        }
    }
    
    bool isConnected(int x, int y) {
        return find(x) == find(y);
    }
};

class Solution {
public:
    void solve(vector<vector<char>>& board) {
        if (board.empty()) return;

        int m = board.size();
        int n = board[0].size();
        int dummyNode = m * n;

        UnionFind uf(m * n + 1);

        // Traverse the board and union border 'O's with dummyNode
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == 'O') {
                    if (i == 0 || i == m - 1 || j == 0 || j == n - 1) {
                        uf.unionSet(i * n + j, dummyNode);
                    } else {
                        if (i > 0 && board[i - 1][j] == 'O') uf.unionSet(i * n + j, (i - 1) * n + j);
                        if (i < m - 1 && board[i + 1][j] == 'O') uf.unionSet(i * n + j, (i + 1) * n + j);
                        if (j > 0 && board[i][j - 1] == 'O') uf.unionSet(i * n + j, i * n + j - 1);
                        if (j < n - 1 && board[i][j + 1] == 'O') uf.unionSet(i * n + j, i * n + j + 1);
                    }
                }
            }
        }

        // Second pass to flip 'O' to 'X' where not connected to the boundary
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == 'O' && !uf.isConnected(dummyNode, i * n + j)) {
                    board[i][j] = 'X';
                }
            }
        }
    }
};
```

#### Time Complexity
- The time complexity here is approximately O(m * n * α(n)), where α is the inverse Ackermann function, due to the union-find operations.

#### Space Complexity
- O(m * n) to maintain the union-find data structures.

All these approaches ensure that any 'O' that should not be converted holds its state and regions that are completely surrounded inside the board get flipped correctly.

