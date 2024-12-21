# [Leetcode 778: Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)

## Approaches:
1. [Brute Force BFS](#approach-1-brute-force-bfs)
2. [Min-Heap / Dijkstra-like Approach](#approach-2-min-heap-dijkstra-like-approach)

---

## Approach 1: Brute Force BFS

### Intuition:
In this approach, we attempt to simulate the swim by checking all possible paths and determining which is feasible at each water level from 0 to `n*n - 1`. We perform Breadth-First Search (BFS) starting from the top-left cell and traverse to adjacent cells provided the water level permits it.

### Steps:
1. Initialize a grid of the same size to mark visited cells.
2. For each possible water level from 0 to `n*n - 1`, check if a path exists from the top-left to the bottom-right corner using BFS.
3. During BFS, only move to a neighboring cell if it is within the bounds, not yet visited, and has a grid value less than or equal to the current water level.
4. If you successfully reach the bottom-right corner, return the current water level.

### Code:

```cpp
#include <vector>
#include <queue>

using namespace std;

class Solution {
public:
    bool canReach(vector<vector<int>>& grid, int threshold) {
        int n = grid.size();
        vector<vector<bool>> visited(n, vector<bool>(n, false));
        queue<pair<int, int>> q;
        q.push({0, 0});
        visited[0][0] = true;
        int directions[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        
        while (!q.empty()) {
            auto [x, y] = q.front();
            q.pop();
            if (x == n - 1 && y == n - 1) return true;

            for (auto& dir : directions) {
                int nx = x + dir[0], ny = y + dir[1];
                if (nx >= 0 && ny >= 0 && nx < n && ny < n && !visited[nx][ny] && grid[nx][ny] <= threshold) {
                    visited[nx][ny] = true;
                    q.push({nx, ny});
                }
            }
        }
        return false;
    }

    int swimInWater(vector<vector<int>>& grid) {
        int n = grid.size();
        for (int t = 0; t < n * n; ++t) {
            if (canReach(grid, t)) {
                return t;
            }
        }
        return -1; // This line should theoretically never be reached.
    }
};
```

### Time Complexity:
- **O(N^3):** We're checking all water levels and performing BFS for each, leading to an `O(N^3)` complexity for an `N x N` grid.

### Space Complexity:
- **O(N^2):** We use extra space for the visited grid and the BFS queue.

---

## Approach 2: Min-Heap / Dijkstra-like Approach

### Intuition:
This approach leverages a priority queue (min-heap) to always try the path with the lowest current maximum elevation. By starting from the initial point, we explore cells using the minimal elevation level required to reach new cells, similar to Dijkstra's shortest path algorithm.

### Steps:
1. Use a min-heap to manage cells, where each entry in the heap is `(water level elevation, x-coordinate, y-coordinate)`.
2. Begin with the starting point `(grid[0][0], 0, 0)` and mark it visited.
3. Continuously extract the minimum elevation cell from the heap and explore its neighbors.
4. Push neighbor cells into the heap if they haven't been visited.
5. Stop when reaching the bottom-right corner, returning the highest elevation level needed on this path.

### Code:

```cpp
#include <vector>
#include <queue>
#include <tuple>

using namespace std;

class Solution {
public:
    int swimInWater(vector<vector<int>>& grid) {
        int n = grid.size();
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<tuple<int, int, int>>> pq;
        vector<vector<bool>> visited(n, vector<bool>(n, false));
        pq.emplace(grid[0][0], 0, 0);
        visited[0][0] = true;
        int directions[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

        while (!pq.empty()) {
            auto [max_elevation, x, y] = pq.top();
            pq.pop();

            if (x == n - 1 && y == n - 1) {
                return max_elevation;
            }

            for (auto& dir : directions) {
                int nx = x + dir[0], ny = y + dir[1];
                if (nx >= 0 && ny >= 0 && nx < n && ny < n && !visited[nx][ny]) {
                    visited[nx][ny] = true;
                    pq.emplace(max(max_elevation, grid[nx][ny]), nx, ny);
                }
            }
        }
        return -1; // This line should theoretically never be reached, as a path should always exist.
    }
};
```

### Time Complexity:
- **O(N^2 log N):** With Dijkstra-like approach, where `N^2` cells are processed and each operation in the priority queue takes logarithmic time.

### Space Complexity:
- **O(N^2):** Similar space used for visited matrix and priority queue, both proportional to the number of cells.

