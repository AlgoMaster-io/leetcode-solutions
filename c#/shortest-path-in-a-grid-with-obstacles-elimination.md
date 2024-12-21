# Leetcode 1293: Shortest Path in a Grid with Obstacles Elimination

[Problem Link](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

## Approaches
- [Approach 1: BFS with Obstacle Tracking](#approach-1-bfs-with-obstacle-tracking)

### Approach 1: BFS with Obstacle Tracking

#### Intuition
The problem is essentially about finding the shortest path in a grid from the top-left corner to the bottom-right corner, but with the added complexity of having obstacles that can be eliminated a limited number of times.

To tackle this, a Breadth-First Search (BFS) is well-suited as it explores all positions layer-by-layer and finds the shortest path in an unweighted grid. Here, we'll keep track of the number of obstacles we have eliminated along with our position in the grid, using a 3D visited array to ensure each state (i.e., position and eliminations) is visited only once.

#### Detailed Steps
1. We will use BFS to explore each cell, using a queue to keep track of the current cell (i, j), the current path length, and the number of obstacles eliminated so far.
2. As we explore each cell, if it's an obstacle, we decide if we can eliminate it by comparing the number of eliminations left.
3. Only continue from a cell if it has not been visited with an equal or greater number of eliminations left than the current path.
4. The BFS will naturally ensure the first time we reach the bottom-right corner of the grid will be using the shortest path.

#### Code
```csharp
public class Solution {
    public int ShortestPath(int[][] grid, int k) {
        int m = grid.Length;
        int n = grid[0].Length;
        
        // Directions for moving in the grid
        int[][] directions = new int[][] {
            new int[] {0, 1},  // right
            new int[] {1, 0},  // down
            new int[] {0, -1}, // left
            new int[] {-1, 0}  // up
        };
        
        // BFS queue, storing (row, col, number of steps, eliminations used)
        Queue<(int, int, int, int)> queue = new Queue<(int, int, int, int)>();
        queue.Enqueue((0, 0, 0, 0));
        
        // Visited array: visited[i][j] = number of obstacles eliminated
        int[][][] visited = new int[m][][];
        for (int i = 0; i < m; i++) {
            visited[i] = new int[n][];
            for (int j = 0; j < n; j++) {
                visited[i][j] = new int[k + 1];
                Array.Fill(visited[i][j], int.MaxValue);
            }
        }
        
        // Start with position (0,0) and 0 eliminations
        visited[0][0][0] = 0;
        
        while (queue.Count > 0) {
            var (x, y, steps, eliminations) = queue.Dequeue();
            
            // Check if reached the bottom-right corner
            if (x == m - 1 && y == n - 1) {
                return steps;
            }
            
            // Explore all 4 possible directions
            foreach (var direction in directions) {
                int nx = x + direction[0];
                int ny = y + direction[1];
                
                // Check boundaries
                if (nx >= 0 && ny >= 0 && nx < m && ny < n) {
                    // Calculate new eliminations count if this new cell is an obstacle
                    int newEliminations = eliminations + (grid[nx][ny] == 1 ? 1 : 0);
                    
                    // Only proceed if newEliminations do not exceed k and no better path has already visited the position
                    if (newEliminations <= k && visited[nx][ny][newEliminations] > steps + 1) {
                        visited[nx][ny][newEliminations] = steps + 1;
                        queue.Enqueue((nx, ny, steps + 1, newEliminations));
                    }
                }
            }
        }
        
        // If we exhaust the queue without finding a path, return -1
        return -1;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(m \times n \times k)\) where \(m\) is the number of rows and \(n\) is the number of columns. Each state (i.e., cell with a specific number of eliminations used) is processed once.
- **Space Complexity:** \(O(m \times n \times k)\) due to the visited array and queue that need to track each state.

This approach efficiently finds the shortest path while respecting the obstacle elimination rules.

