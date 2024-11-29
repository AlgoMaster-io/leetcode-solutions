# 994. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

## Approach: Breadth-First Search (BFS)

### Solution
```cpp
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the queue and visited cells
#include <vector>
#include <queue>
using namespace std;

class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();
        queue<pair<int, int>> queue;
        int freshCount = 0;

        // Step 1: Initialize the queue with all rotten oranges and count fresh oranges
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 2) {
                    queue.push({ i, j });
                } else if (grid[i][j] == 1) {
                    freshCount++;
                }
            }
        }

        // Step 2: Perform BFS to rot adjacent fresh oranges
        int minutes = 0;
        vector<vector<int>> directions = { { 0, 1 }, { 1, 0 }, { 0, -1 }, { -1, 0 } };

        while (!queue.empty() && freshCount > 0) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                auto [currentRow, currentCol] = queue.front();
                queue.pop();
                for (auto& dir : directions) {
                    int newRow = currentRow + dir[0];
                    int newCol = currentCol + dir[1];

                    // Check bounds and if the orange is fresh
                    if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && grid[newRow][newCol] == 1) {
                        grid[newRow][newCol] = 2; // Rot the orange
                        queue.push({ newRow, newCol });
                        freshCount--;
                    }
                }
            }
            minutes++;
        }

        // If there are still fresh oranges left, return -1
        return freshCount == 0 ? minutes : -1;
    }
};
```

