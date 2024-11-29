# 329. [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approach 1: DFS with Memoization

### Solution
```cpp
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
private:
    vector<vector<int>> directions{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}; // 4 possible directions: right, down, left, up

public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return 0;
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> memo(m, vector<int>(n, 0)); // Memoization array to store results of subproblems
        int longestPath = 0;

        // Explore each cell in the matrix using DFS
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                longestPath = max(longestPath, dfs(matrix, i, j, memo));
            }
        }

        return longestPath;
    }

private:
    int dfs(vector<vector<int>>& matrix, int i, int j, vector<vector<int>>& memo) {
        if (memo[i][j] != 0) return memo[i][j]; // Return cached result if already computed

        int maxPath = 1; // Minimum path length is 1 (the cell itself)

        // Explore all four directions
        for (auto& dir : directions) {
            int x = i + dir[0], y = j + dir[1];
            if (x >= 0 && x < matrix.size() && y >= 0 && y < matrix[0].size() && matrix[x][y] > matrix[i][j]) {
                int length = 1 + dfs(matrix, x, y, memo);
                maxPath = max(maxPath, length);
            }
        }

        memo[i][j] = maxPath; // Cache the result before returning

        return maxPath;
    }
};
```

## Approach 2: Topological Sort with Indegree

### Solution
```cpp
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
#include <vector>
#include <queue>
using namespace std;

class Solution {
private:
    vector<vector<int>> directions{{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return 0;
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> indegree(m, vector<int>(n, 0)); // Array to store number of incoming edges

        // Calculate indegrees of each cell
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (auto& dir : directions) {
                    int x = i + dir[0], y = j + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[i][j]) {
                        indegree[x][y]++;
                    }
                }
            }
        }

        queue<pair<int, int>> q;
        // Initialize queue with cells having 0 indegree
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (indegree[i][j] == 0) {
                    q.emplace(i, j);
                }
            }
        }

        int pathLength = 0;
        // Process each cell in the queue
        while (!q.empty()) {
            int size = q.size();
            pathLength++;
            for (int k = 0; k < size; k++) {
                auto cell = q.front();
                q.pop();
                for (auto& dir : directions) {
                    int x = cell.first + dir[0], y = cell.second + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[cell.first][cell.second]) {
                        indegree[x][y]--;
                        if (indegree[x][y] == 0) {
                            q.emplace(x, y);
                        }
                    }
                }
            }
        }

        return pathLength;
    }
};
```

