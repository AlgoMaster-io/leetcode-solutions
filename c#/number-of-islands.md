## [Leetcode 200: Number of Islands](https://leetcode.com/problems/number-of-islands/)

### Navigation
- [Approach 1: Depth-First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Union-Find](#approach-3-union-find)

### Approach 1: Depth-First Search (DFS)

**Intuition:**
The problem involves finding connected components (islands) in a 2D grid, which can be represented as a graph. Each cell is a node, and edges exist between all 4-directionally adjacent land cells (isolated water cells are not part of the graph).

The DFS approach involves visiting all nodes connected to a starting land cell, marking them as visited (or turning them into water), and counting how many times we start a new DFS from an unvisited land cell.

**Steps:**
1. Iterate over each cell in the grid.
2. Start a DFS when you encounter an unvisited land cell (marked as '1').
3. Use DFS to mark the entire connected component (island) as visited (turn '1's into '0's).
4. Increment the island count each time a DFS initiates from a land cell.

**Code:**

```csharp
public class Solution {
    public int NumIslands(char[][] grid) {
        if(grid == null || grid.Length == 0) return 0;
        int count = 0;
        for(int i = 0; i < grid.Length; i++) {
            for(int j = 0; j < grid[0].Length; j++) {
                if(grid[i][j] == '1') { // Start a DFS from each unvisited land cell
                    DFS(grid, i, j);
                    count++; // Increment the count of islands
                }
            }
        }
        return count;
    }
    
    private void DFS(char[][] grid, int i, int j) {
        // If the cell is out of bounds or it is water, return
        if(i < 0 || i >= grid.Length || j < 0 || j >= grid[0].Length || grid[i][j] == '0') return;
        
        grid[i][j] = '0'; // Mark the current cell as water to indicate it's visited
        
        // Explore all four directions (up, down, left, right)
        DFS(grid, i + 1, j);
        DFS(grid, i - 1, j);
        DFS(grid, i, j + 1);
        DFS(grid, i, j - 1);
    }
}
```

**Time Complexity:** O(M * N) where M is the number of rows and N is the number of columns. Every cell is checked once.

**Space Complexity:** O(M * N) in the worst case for the recursion stack.

### Approach 2: Breadth-First Search (BFS)

**Intuition:**
Similar to DFS, BFS can also be used to traverse the island components. The idea is to use a queue to explore all nodes at the present depth prior to moving on to nodes at the next depth level.

**Steps:**
1. Use a queue to perform BFS starting from each unvisited land cell.
2. Mark all reachable land cells as visited by turning them into '0's.
3. Increment the island count for each BFS initiated.

**Code:**

```csharp
public class Solution {
    public int NumIslands(char[][] grid) {
        if(grid == null || grid.Length == 0) return 0;
        int count = 0;
        for(int i = 0; i < grid.Length; i++) {
            for(int j = 0; j < grid[0].Length; j++) {
                if(grid[i][j] == '1') {
                    BFS(grid, i, j);
                    count++;
                }
            }
        }
        return count;
    }
    
    private void BFS(char[][] grid, int x, int y) {
        Queue<(int, int)> queue = new Queue<(int, int)>();
        queue.Enqueue((x, y));
        grid[x][y] = '0'; // Mark starting cell as visited
        int[][] directions = new int[][] {
            new int[]{1, 0}, new int[]{-1, 0}, new int[]{0, 1}, new int[]{0, -1}
        };
        
        while(queue.Count > 0) {
            var (i, j) = queue.Dequeue();
            foreach(var direction in directions) {
                int r = i + direction[0];
                int c = j + direction[1];
                if(r >= 0 && r < grid.Length && c >= 0 && c < grid[0].Length && grid[r][c] == '1') {
                    queue.Enqueue((r, c));
                    grid[r][c] = '0'; // Mark as visited
                }
            }
        }
    }
}
```

**Time Complexity:** O(M * N), each cell is processed once.

**Space Complexity:** O(min(M, N)) for the width of the queue in the worst case.

### Approach 3: Union-Find

**Intuition:**
Union-Find (Disjoint Set Union, DSU) is a data structure that can efficiently perform operations to determine if two elements are in the same set, and to unite two sets. 

For the grid, treat each cell as an element, and union cells if they are adjacent and both are land. The number of sets (or connected components) is the number of islands.

**Code:**

```csharp
public class Solution {
    public int NumIslands(char[][] grid) {
        if(grid == null || grid.Length == 0) return 0;
        int rows = grid.Length;
        int cols = grid[0].Length;
        int[] parent = new int[rows * cols];
        int count = 0;
        
        for(int r = 0; r < rows; r++) {
            for(int c = 0; c < cols; c++) {
                if(grid[r][c] == '1') {
                    parent[r * cols + c] = r * cols + c;
                    count++;
                }
            }
        }
        
        for(int r = 0; r < rows; r++) {
            for(int c = 0; c < cols; c++) {
                if(grid[r][c] == '1') {
                    foreach(var (dr, dc) in new List<(int, int)>{(1, 0), (0, 1)}) {
                        int nr = r + dr, nc = c + dc;
                        if(nr < rows && nc < cols && grid[nr][nc] == '1') {
                            if(Union(r * cols + c, nr * cols + nc, parent)) {
                                count--;
                            }
                        }
                    }
                }
            }
        }
        return count;
    }
    
    private int Find(int i, int[] parent) {
        if(parent[i] != i) {
            parent[i] = Find(parent[i], parent); // Path compression
        }
        return parent[i];
    }
    
    private bool Union(int x, int y, int[] parent) {
        int rootX = Find(x, parent);
        int rootY = Find(y, parent);
        if(rootX != rootY) {
            parent[rootX] = rootY;
            return true;
        }
        return false;
    }
}
```

**Time Complexity:** O(M * N * α(M * N)), where α is the Inverse Ackermann function, which is nearly constant.

**Space Complexity:** O(M * N) for the parent array.

