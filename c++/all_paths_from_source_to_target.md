# 797. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approach: Depth-First Search (DFS)

### Solution
```cpp
// Time Complexity: O(2^n * n), where n is the number of nodes in the graph
// Space Complexity: O(2^n * n), for storing all paths in the result
#include <vector>

class Solution {
public:
    std::vector<std::vector<int>> allPathsSourceTarget(std::vector<std::vector<int>>& graph) {
        std::vector<std::vector<int>> result;
        std::vector<int> path;
        path.push_back(0); // Start from the source node
        dfs(graph, 0, path, result);
        return result;
    }

private:
    void dfs(std::vector<std::vector<int>>& graph, int node, std::vector<int>& path, std::vector<std::vector<int>>& result) {
        if (node == graph.size() - 1) {
            // If we reach the target node, add the current path to the result
            result.push_back(path);
            return;
        }

        for (int neighbor : graph[node]) {
            path.push_back(neighbor); // Add the neighbor to the current path
            dfs(graph, neighbor, path, result); // Recur for the neighbor
            path.pop_back(); // Backtrack to explore other paths
        }
    }
};
```

