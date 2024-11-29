# 847. [Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Approach 1: Breadth-First Search (BFS) with Bitmask

### Solution
```cpp
// Time Complexity: O(2^n * n^2)
// Space Complexity: O(2^n * n)

#include <vector>
#include <queue>
#include <array>

using namespace std;

class Solution {
public:
    int shortestPathLength(vector<vector<int>>& graph) {
        int n = graph.size();
        if (n == 1) return 0;

        // Queue for BFS, storing {node, visited nodes mask}
        queue<array<int, 2>> queue;
        vector<vector<bool>> visited(n, vector<bool>(1 << n, false));

        // Initialize queue and visited set with all starting nodes
        for (int i = 0; i < n; ++i) {
            queue.push({i, 1 << i});
            visited[i][1 << i] = true;
        }

        int steps = 0;
        while (!queue.empty()) {
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                auto current = queue.front();
                queue.pop();
                int node = current[0], mask = current[1];

                // If all nodes are visited
                if (mask == (1 << n) - 1) return steps;

                // Visit all the neighbors
                for (int nei : graph[node]) {
                    int nextMask = mask | (1 << nei);
                    if (!visited[nei][nextMask]) {
                        queue.push({nei, nextMask});
                        visited[nei][nextMask] = true;
                    }
                }
            }
            steps++;
        }
        return -1;
    }
};
```

## Approach 2: Dynamic Programming with Bitmask

### Solution
```cpp
// Time Complexity: O(2^n * n^2)
// Space Complexity: O(2^n * n)

#include <vector>
#include <queue>
#include <array>
#include <algorithm>

using namespace std;

class Solution {
public:
    int shortestPathLength(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<vector<int>> dp(1 << n, vector<int>(n, INT_MAX));

        queue<array<int, 2>> queue;
        for (int i = 0; i < n; ++i) {
            queue.push({1 << i, i});
            dp[1 << i][i] = 0;
        }

        while (!queue.empty()) {
            auto current = queue.front();
            queue.pop();
            int mask = current[0], node = current[1];

            for (int nei : graph[node]) {
                int nextMask = mask | (1 << nei);
                if (dp[nextMask][nei] > dp[mask][node] + 1) {
                    dp[nextMask][nei] = dp[mask][node] + 1;
                    queue.push({nextMask, nei});
                }
            }
        }

        int result = INT_MAX;
        for (int i = 0; i < n; ++i) {
            result = min(result, dp[(1 << n) - 1][i]);
        }
        return result;
    }
};
```

Both approaches use BFS and dynamic programming with bitmask to efficiently find the shortest path to visit all nodes, ensuring optimality in solving the problem.

