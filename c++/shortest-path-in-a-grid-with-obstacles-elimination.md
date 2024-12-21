# [1293. Shortest Path in a Grid with Obstacles Elimination](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

## Approaches:
- [Approach 1: Breadth-First Search (BFS) with State Tracking](#approach-1-breadth-first-search-bfs-with-state-tracking)

### Approach 1: Breadth-First Search (BFS) with State Tracking

**Intuition:**

The problem is essentially asking for the shortest path in a grid. The catch is we can eliminate up to `k` obstacles. This is a stateful BFS problem where we must track not only the current position but also the number of obstacles we've eliminated so far.

The BFS approach is chosen because it naturally finds the shortest path in an unweighted grid when obstacles or states do not need to be considered. Here, with states added by the obstacle eliminations, the BFS can be extended with additional state information.

**Steps:**

1. Use a queue to perform BFS. The state stored in the queue will be the current position `(x, y)`, the number of steps taken so far, and the number of obstacles eliminated.
2. Use a 3D vector to track if a particular cell has been visited with a specific number of remaining obstacle eliminations.
3. Begin by inserting the starting position `(0, 0)` into the queue with zero steps and zero obstacles eliminated.
4. For each position, explore its 4 possible neighbors (up, down, left, right).
5. For each neighbor:
   - If it's the destination, return the steps + 1.
   - If it's a valid cell:
     - Calculate the new number of obstacles eliminated if the cell contains an obstacle.
     - If the new elimination count does not exceed `k` and this cell has not been visited with this elimination count, add the neighbor to the queue.

6. If the BFS completes without reaching the target, return `-1`, which means it's not possible to reach the destination within the allowed obstacle eliminations.

**Time Complexity:**
- O(n * m * k): We explore up to each cell with every state of obstacle eliminations up to `k`.

**Space Complexity:**
- O(n * m * k): The 3D vector and queue could potentially store up to this many states.

```cpp
#include <vector>
#include <queue>
using namespace std;

int shortestPath(vector<vector<int>>& grid, int k) {
    int n = grid.size(), m = grid[0].size();
    vector<vector<vector<bool>>> visited(n, vector<vector<bool>>(m, vector<bool>(k + 1, false)));
    
    // Directions array for moving in the 4 possible directions.
    vector<pair<int, int>> directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    
    // BFS queue: stores (row, column, current path length, obstacles eliminated)
    queue<tuple<int, int, int, int>> q;
    q.push({0, 0, 0, 0});
    
    while (!q.empty()) {
        auto [x, y, steps, eliminated] = q.front();
        q.pop();
        
        // If we've reached the destination, return the number of steps
        if (x == n - 1 && y == m - 1) {
            return steps;
        }
        
        // Try moving in all 4 directions
        for (auto [dx, dy] : directions) {
            int nx = x + dx, ny = y + dy;
            
            // Check within boundaries
            if (nx >= 0 && nx < n && ny >= 0 && ny < m) {
                int newEliminated = eliminated + grid[nx][ny]; // increment obstacles eliminated if it's an obstacle
                
                // If it is feasible to visit and this state is unvisited
                if (newEliminated <= k && !visited[nx][ny][newEliminated]) {
                    visited[nx][ny][newEliminated] = true;
                    q.push({nx, ny, steps + 1, newEliminated});
                }
            }
        }
    }
    
    // If we exit the loop, no valid path to destination was found
    return -1;
}
```

In this solution, BFS naturally explores the shortest paths first, and by keeping track of how many obstacles we have eliminated in the state, we ensure that we consider all paths that could potentially lead to the destination within the allowed parameters.

