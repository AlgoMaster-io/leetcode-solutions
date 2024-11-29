# 542. [01 Matrix](https://leetcode.com/problems/01-matrix/)

## Approach: Breadth-First Search (BFS)

### Solution
```cpp
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the queue and distance matrix
#include <vector>
#include <queue>

using namespace std;

class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        int rows = mat.size();
        int cols = mat[0].size();
        vector<vector<int>> distances(rows, vector<int>(cols, 0));
        vector<vector<bool>> visited(rows, vector<bool>(cols, false));
        queue<pair<int, int>> queue;

        // Step 1: Initialize the queue with all '0' cells and mark them as visited
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (mat[i][j] == 0) {
                    queue.push({i, j});
                    visited[i][j] = true;
                }
            }
        }

        // Step 2: Perform BFS to calculate distances
        vector<vector<int>> directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        while (!queue.empty()) {
            auto current = queue.front();
            queue.pop();
            for (auto dir : directions) {
                int newRow = current.first + dir[0];
                int newCol = current.second + dir[1];

                // Check bounds and if the cell is not visited
                if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && !visited[newRow][newCol]) {
                    distances[newRow][newCol] = distances[current.first][current.second] + 1;
                    queue.push({newRow, newCol});
                    visited[newRow][newCol] = true;
                }
            }
        }

        return distances;
    }
};
```

