# [Leetcode 1631: Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Solutions:
1. [Dijkstra's Algorithm with Priority Queue](#dijkstras-algorithm-with-priority-queue)
2. [Binary Search with BFS/DFS](#binary-search-with-bfsdfs)

---

### Dijkstra's Algorithm with Priority Queue

The problem can be visualized as finding a path in a weighted grid where the weight of each edge is represented by the absolute difference in heights between two connected cells. Our objective is to find a path where the maximum weight in our path is minimized.

#### Intuition:
Dijkstra's algorithm is a classic approach to find the shortest path in a weighted graph. In this context, "shortest path" means the path where the maximum weight (effort) is minimized among all possible paths from the top-left corner to the bottom-right corner of the grid.

- We'll use a priority queue to always extend the path which has the currently smallest maximum effort.
- The state in the priority queue includes the maximum effort encountered so far to reach a specific cell.
- We iteratively explore cells in increasing order of their path effort.

#### Steps:
1. Create a min-heap (priority queue) to store the state `(effort, x, y)`.
2. Use a 2D vector to track minimum efforts to reach each cell from the starting point.
3. Initialize the heap with the starting cell `(0, 0, 0)` meaning zero effort to start.
4. Perform breadth-first search (BFS) using the priority queue:
   - Extract the cell with the minimum effort.
   - If it is the target cell, return the effort.
   - For each neighbor of the current cell, calculate the effort and update the minimum effort if possible. Push the new state to the heap if worth exploring.

```cpp
#include <vector>
#include <queue>
#include <algorithm>
#include <cmath>

using namespace std;

int minimumEffortPath(vector<vector<int>>& heights) {
    int rows = heights.size();
    int cols = heights[0].size();
    vector<vector<int>> effort(rows, vector<int>(cols, INT_MAX));
    priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<>> pq;
    
    const vector<int> directions = {-1, 0, 1, 0, -1};
    
    pq.emplace(0, 0, 0); // (effort, x, y)
    effort[0][0] = 0;
    
    while (!pq.empty()) {
        auto [currEffort, x, y] = pq.top();
        pq.pop();
        
        // If reached the end, return the current effort
        if (x == rows - 1 && y == cols - 1) {
            return currEffort;
        }
        
        for (int i = 0; i < 4; ++i) {
            int nx = x + directions[i];
            int ny = y + directions[i + 1];
            
            // Check bounds
            if (nx >= 0 && ny >= 0 && nx < rows && ny < cols) {
                // Calculate the effort to reach the neighbour
                int nextEffort = max(currEffort, abs(heights[nx][ny] - heights[x][y]));
                // If this path offers a more optimal way to the neighbour
                if (nextEffort < effort[nx][ny]) {
                    effort[nx][ny] = nextEffort;
                    pq.emplace(nextEffort, nx, ny);
                }
            }
        }
    }
    
    return 0;
}
```
- **Time Complexity:** \(O(M \times N \log(M \times N))\), where \(M\) is the number of rows and \(N\) is the number of columns. Each cell is visited and processed in the priority queue.
- **Space Complexity:** \(O(M \times N)\), for storing efforts and the priority queue.

### Binary Search with BFS/DFS

#### Intuition:
Instead of exploring paths in order of effort, we can approach this as a decision problem. Can we determine if a path exists such that all segments have an effort less than or equal to some threshold? Using binary search on this threshold combined with BFS/DFS, we can efficiently find the minimum possible effort.

- The search space is the effort, ranging from 0 to the maximum possible difference in heights between any two adjacent cells.
- For a given mid-point in binary search, use BFS/DFS to check if a path exists that respects this maximum effort.

#### Steps:
1. Determine the initial binary search bounds: \(low = 0\) and \(high = \max(\text{all heights differences})\).
2. Perform binary search:
   - For each middle (`mid`) value of effort, use BFS/DFS to check if the path can be completed with all segments having effort ≤ mid.
   - Adjust bounds accordingly.
3. Continue until the bounds converge.

```cpp
#include <vector>
#include <queue>

using namespace std;

bool canReachEnd(vector<vector<int>>& heights, int maxEffort) {
    int rows = heights.size();
    int cols = heights[0].size();
    vector<vector<bool>> visited(rows, vector<bool>(cols, false));
    queue<pair<int, int>> q;
    q.push({0, 0});
    visited[0][0] = true;
    
    const vector<int> directions = {-1, 0, 1, 0, -1};
    
    while (!q.empty()) {
        auto [x, y] = q.front();
        q.pop();
        
        if (x == rows - 1 && y == cols - 1) {
            return true;
        }
        
        for (int i = 0; i < 4; ++i) {
            int nx = x + directions[i];
            int ny = y + directions[i + 1];
            // Check bounds and if visited
            if (nx >= 0 && ny >= 0 && nx < rows && ny < cols && !visited[nx][ny]) {
                // Check if the current path with the given effort is feasible
                if (abs(heights[nx][ny] - heights[x][y]) <= maxEffort) {
                    visited[nx][ny] = true;
                    q.push({nx, ny});
                }
            }
        }
    }
    
    return false;
}

int minimumEffortPath(vector<vector<int>>& heights) {
    int low = 0, high = 0;
    for (int i = 0; i < heights.size(); ++i) {
        for (int j = 0; j < heights[i].size(); ++j) {
            if (i > 0) high = max(high, abs(heights[i][j] - heights[i-1][j]));
            if (j > 0) high = max(high, abs(heights[i][j] - heights[i][j-1]));
        }
    }
    
    int result = high;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (canReachEnd(heights, mid)) {
            result = mid;
            high = mid - 1; // Try for a smaller effort
        } else {
            low = mid + 1; // Increase the acceptable effort
        }
    }
    
    return result;
}
```
- **Time Complexity:** \(O(\log(D) \times M \times N)\), where \(D\) is the maximum difference between heights among all adjacent cells.
- **Space Complexity:** \(O(M \times N)\), for the BFS/DFS queue and the visited matrix. 

Each of these approaches offers a unique perspective on solving the problem, with the first one being more akin to classical shortest path algorithms and the second leveraging binary search and decision making to tackle the problem efficiently.

