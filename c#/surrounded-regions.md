# [Leetcode 130: Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

## Approaches

1. [DFS Approach](#dfs-approach)
2. [Union-Find Approach](#union-find-approach)

### DFS Approach

The problem asks us to capture all regions surrounded by 'X'. A region is captured by flipping all 'O's into 'X's.

#### Intuition

- The main idea is that any 'O' connected directly to the boundary of the board cannot be flipped.
- We can use Depth First Search (DFS) to mark all such 'O's. 
- After we've marked all the 'O's that cannot be flipped, flip the remaining 'O's as they are surrounded by 'X's.

#### Steps

1. Traverse every cell on the border of the board.
2. If a border cell contains 'O', initiate a DFS to mark all connected 'O's starting from that cell.
3. After marking, traverse all cells on the board:
   - Flip all marked 'O's back to 'O'.
   - Flip all remaining 'O's to 'X' since they are surrounded.
4. The "marking" can be done by changing 'O' to a temporary value, like '#', to differentiate the marked cells from unmarked ones.

#### Code

```csharp
public class Solution {
    public void Solve(char[][] board) {
        if (board == null || board.Length == 0) return;
        
        int rows = board.Length;
        int cols = board[0].Length;
        
        for (int i = 0; i < rows; i++) {
            DFS(board, i, 0); // Left border
            DFS(board, i, cols - 1); // Right border
        }
        
        for (int j = 0; j < cols; j++) {
            DFS(board, 0, j); // Top border
            DFS(board, rows - 1, j); // Bottom border
        }
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'X'; // Surrounded region
                }
                else if (board[i][j] == '#') {
                    board[i][j] = 'O'; // Non-surrounded region
                }
            }
        }
    }
    
    private void DFS(char[][] board, int i, int j) {
        if (i < 0 || i >= board.Length || j < 0 || j >= board[0].Length || board[i][j] != 'O') return;
        
        board[i][j] = '#'; // Mark the cell as visited
        
        DFS(board, i - 1, j); // Up
        DFS(board, i + 1, j); // Down
        DFS(board, i, j - 1); // Left
        DFS(board, i, j + 1); // Right
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(m * n), because we may need to visit all the cells in the worst case.
- **Space Complexity**: O(m * n), which is the space used for the recursive stack in the worst case.

### Union-Find Approach

This approach avoids recursive depth limitations by using the Union-Find data structure. It's slightly complex but worth understanding for better grasp on connectivity problems.

#### Intuition

1. Extend the usual grid by including a virtual node connected to all border 'O's.
2. Use a Union-Find data structure to connect all 'O's directly adjacent to the boundary and subsequently to each other.
3. After partitioning, iterate through the grid to flip 'O's to 'X' if they are not connected to the virtual node.

#### Code

```csharp
public class Solution {
    public void Solve(char[][] board) {
        if (board == null || board.Length == 0) return;

        int rows = board.Length;
        int cols = board[0].Length;
        UnionFind uf = new UnionFind(rows * cols + 1);
        int dummyNode = rows * cols;

        // Connect 'O's on the border to the dummyNode
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if ((i == 0 || j == 0 || i == rows - 1 || j == cols - 1) && board[i][j] == 'O') {
                    uf.Union(i * cols + j, dummyNode);
                }
            }
        }

        // Connect inner 'O's together
        int[][] directions = new int[][] {
            new int[] {1, 0}, new int[] {0, 1}, new int[] {-1, 0}, new int[] {0, -1}
        };

        for (int i = 1; i < rows - 1; i++) {
            for (int j = 1; j < cols - 1; j++) {
                if (board[i][j] == 'O') {
                    foreach (var dir in directions) {
                        int x = i + dir[0], y = j + dir[1];
                        if (board[x][y] == 'O') {
                            uf.Union(i * cols + j, x * cols + y);
                        }
                    }
                }
            }
        }

        // Flip all non-connected 'O's to 'X'
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (board[i][j] == 'O' && !uf.IsConnected(i * cols + j, dummyNode)) {
                    board[i][j] = 'X';
                }
            }
        }
    }
}

public class UnionFind {
    private int[] parent;
    
    public UnionFind(int size) {
        parent = new int[size];
        for (int i = 0; i < size; i++) {
            parent[i] = i;
        }
    }
    
    public int Find(int x) {
        if (parent[x] != x) {
            parent[x] = Find(parent[x]);
        }
        return parent[x];
    }
    
    public void Union(int x, int y) {
        int rootX = Find(x);
        int rootY = Find(y);
        if (rootX != rootY) {
            parent[rootX] = rootY;
        }
    }
    
    public bool IsConnected(int x, int y) {
        return Find(x) == Find(y);
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(m * n * α(n)), where α is the Inverse Ackermann function, very slow growing, practically constant.
- **Space Complexity**: O(m * n), consumed mainly by the Union-Find parent array.

