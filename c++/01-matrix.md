**[Leetcode Problem 542: 01 Matrix](https://leetcode.com/problems/01-matrix/)**

### Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)
3. [Breadth-First Search (BFS) with Queue](#bfs-with-queue)
4. [Optimized Multi-source BFS](#optimized-multi-source-bfs)

---

#### Brute Force Approach

The brute force approach involves calculating the distance of each cell with value '1' to the nearest cell with value '0' by checking all directions. This results in repeated calculations, making it inefficient for large matrices.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>

using namespace std;

vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
    int rows = mat.size();
    int cols = mat[0].size();
    vector<vector<int>> dist(rows, vector<int>(cols, INT_MAX));
    
    // Iterate through the matrix
    for (int i = 0; i < rows; ++i) {
        for (int j = 0; j < cols; ++j) {
            // Find the distance for each '1' cell
            if (mat[i][j] == 0) {
                dist[i][j] = 0;
            } else {
                // Check all directions
                if (i > 0) dist[i][j] = min(dist[i][j], dist[i-1][j] + 1);
                if (j > 0) dist[i][j] = min(dist[i][j], dist[i][j-1] + 1);
            }
        }
    }
    
    for (int i = rows - 1; i >= 0; --i) {
        for (int j = cols - 1; j >= 0; --j) {
            if (mat[i][j] == 0) continue;
            // Again check all directions but in reverse order
            if (i < rows - 1) dist[i][j] = min(dist[i][j], dist[i+1][j] + 1);
            if (j < cols - 1) dist[i][j] = min(dist[i][j], dist[i][j+1] + 1);
        }
    }
    return dist;
}
```

**Time Complexity**: O((mn)^2), where m and n are the number of rows and columns respectively, because for each cell we iterate through the matrix.

**Space Complexity**: O(mn), where m and n are the dimensions of the matrix, for storing the result.

---

#### Dynamic Programming Approach

Involves calculating the minimal distance by traversing the matrix twice (once from top-left to bottom-right and then from bottom-right to top-left) and updating the distance based on prior calculations.

```cpp
#include <vector>
#include <algorithm>
#include <climits>

using namespace std;

vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
    int rows = mat.size();
    int cols = mat[0].size();
    vector<vector<int>> dist(rows, vector<int>(cols, INT_MAX));
    
    // First pass: top-left to bottom-right
    for (int i = 0; i < rows; ++i) {
        for (int j = 0; j < cols; ++j) {
            if (mat[i][j] == 0) {
                dist[i][j] = 0;
            } else {
                if (i > 0) dist[i][j] = min(dist[i][j], dist[i-1][j] + 1);
                if (j > 0) dist[i][j] = min(dist[i][j], dist[i][j-1] + 1);
            }
        }
    }
    
    // Second pass: bottom-right to top-left
    for (int i = rows - 1; i >= 0; --i) {
        for (int j = cols - 1; j >= 0; --j) {
            if (mat[i][j] == 0) continue;
            if (i < rows - 1) dist[i][j] = min(dist[i][j], dist[i+1][j] + 1);
            if (j < cols - 1) dist[i][j] = min(dist[i][j], dist[i][j+1] + 1);
        }
    }
    
    return dist;
}
```

**Time Complexity**: O(mn), since we traverse the matrix twice.

**Space Complexity**: O(1) additional since we're using the input matrix as our return matrix, apart from minor auxiliary space.

---

#### BFS with Queue

Uses BFS from each zero cell to calculate the distance for all ‘1’ cells.

```cpp
#include <vector>
#include <queue>

using namespace std;

int directions[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
    int m = mat.size(), n = mat[0].size();
    vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
    queue<pair<int, int>> q;

    // Start BFS from all 0 cells
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (mat[i][j] == 0) {
                dist[i][j] = 0;
                q.push({i, j});
            }
        }
    }

    while (!q.empty()) {
        auto [i, j] = q.front();
        q.pop();

        // Explore neighbors
        for (auto& dir : directions) {
            int ni = i + dir[0], nj = j + dir[1];
            // Check if neighboring cell is within bounds and update the distance
            if (ni >= 0 && ni < m && nj >= 0 && nj < n) {
                if (dist[ni][nj] > dist[i][j] + 1) {
                    dist[ni][nj] = dist[i][j] + 1;
                    q.push({ni, nj});
                }
            }
        }
    }

    return dist;
}
```

**Time Complexity**: O(mn), since each cell is processed once.

**Space Complexity**: O(mn) for the queue and distance matrix.

---

#### Optimized Multi-source BFS

This approach uses BFS starting from all the zeroes simultaneously, marking the initial zero cells with distance 0. This prevents backtracking, making it highly efficient.

```cpp
#include <vector>
#include <queue>

using namespace std;

int directions[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
    int m = mat.size(), n = mat[0].size();
    vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
    queue<pair<int, int>> q;

    // Initialize the queue with all 0 cells
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (mat[i][j] == 0) {
                dist[i][j] = 0;
                q.push({i, j});
            }
        }
    }

    while (!q.empty()) {
        auto [i, j] = q.front();
        q.pop();

        // Check all 4 possible directions
        for (auto& dir : directions) {
            int ni = i + dir[0], nj = j + dir[1];
            
            // Verify if neighbor is inbound and update if shorter distance is found
            if (ni >= 0 && ni < m && nj >= 0 && nj < n) {
                if (dist[ni][nj] > dist[i][j] + 1) {
                    dist[ni][nj] = dist[i][j] + 1;
                    q.push({ni, nj});
                }
            }
        }
    }

    return dist;
}
```

**Time Complexity**: O(mn), as each cell is enqueued and dequeued once in BFS.

**Space Complexity**: O(mn), used for the queue to store coordinates.

This problem exemplifies how various techniques from brute-force to optimized BFS can be applied to enhance efficiency and achieve a more optimal solution.

