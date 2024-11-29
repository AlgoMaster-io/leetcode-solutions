# 1631. [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
```cpp
#include <vector>
#include <queue>
#include <limits>
#include <cmath>

using namespace std;

// Time Complexity: O(m * n * log(m * n)), where m is the number of rows and n is the number of columns.
// Space Complexity: O(m * n)
class Solution {
public:
    // Direction vectors for exploring adjacent cells (right, left, down, up)
    static const vector<vector<int>> DIRECTIONS;

    int minimumEffortPath(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        
        // Min-heap priority queue to select the path with the minimum effort so far
        auto compare = [](const vector<int>& a, const vector<int>& b) { return a[2] > b[2]; };
        priority_queue<vector<int>, vector<vector<int>>, decltype(compare)> minHeap(compare);

        // Efforts matrix to track the minimum effort required to reach each cell
        vector<vector<int>> efforts(m, vector<int>(n, numeric_limits<int>::max()));
        efforts[0][0] = 0;

        // Start from the top-left corner of the grid
        minHeap.push({0, 0, 0});

        while (!minHeap.empty()) {
            // Extract the cell with the minimum effort
            vector<int> current = minHeap.top();
            minHeap.pop();
            int x = current[0], y = current[1], currentEffort = current[2];

            // If we've reached the bottom-right corner
            if (x == m - 1 && y == n - 1) {
                return currentEffort;
            }

            // Explore all adjacent cells
            for (const auto& direction : DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                // Check bounds
                if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                    // Calculate the effort required to move to the adjacent cell
                    int effort = max(currentEffort, abs(heights[newX][newY] - heights[x][y]));

                    // If this path to the adjacent cell is better, update efforts and heap
                    if (effort < efforts[newX][newY]) {
                        efforts[newX][newY] = effort;
                        minHeap.push({newX, newY, effort});
                    }
                }
            }
        }

        return 0; // Shouldn't reach here. Always possible to reach the end.
    }
};

const vector<vector<int>> Solution::DIRECTIONS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
```

## Approach 2: Binary Search + Depth First Search (DFS)

### Solution
```cpp
#include <vector>
#include <queue>
#include <cmath>

using namespace std;

// Time Complexity: O(m * n * log(maxDifference))
// Space Complexity: O(m * n)
class Solution {
public:
    // Direction vectors for exploring adjacent cells (right, left, down, up)
    static const vector<vector<int>> DIRECTIONS;

    int minimumEffortPath(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        int left = 0, right = 1'000'000, result = 1'000'000;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (canReachEndWithEffort(heights, mid, m, n)) {
                result = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }

        return result;
    }

private:
    // Helper function to determine if there is a path from (0, 0) to (m - 1, n - 1) under the given maxEffort
    bool canReachEndWithEffort(const vector<vector<int>>& heights, int maxEffort, int m, int n) {
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        queue<pair<int, int>> q;
        q.push({0, 0});
        visited[0][0] = true;

        while (!q.empty()) {
            auto [x, y] = q.front();
            q.pop();

            if (x == m - 1 && y == n - 1) {
                return true;
            }

            for (const auto& direction : DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                if (newX >= 0 && newX < m && newY >= 0 && newY < n && !visited[newX][newY]) {
                    int currentEffort = abs(heights[newX][newY] - heights[x][y]);

                    if (currentEffort <= maxEffort) {
                        visited[newX][newY] = true;
                        q.push({newX, newY});
                    }
                }
            }
        }

        return false;
    }
};

const vector<vector<int>> Solution::DIRECTIONS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
```

