# 684. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approach 1: Union-Find Algorithm

### Solution
```cpp
// Time Complexity: O(n * α(n)), where α is the inverse Ackermann function
// Space Complexity: O(n)
#include <vector>

class Solution {
public:
    std::vector<int> findRedundantConnection(std::vector<std::vector<int>>& edges) {
        int n = edges.size();
        std::vector<int> parent(n + 1);
        std::vector<int> rank(n + 1, 1);

        // Initialize the parent for each node as itself
        for (int i = 1; i <= n; ++i) {
            parent[i] = i;
        }

        for (const auto& edge : edges) {
            // If the two nodes are already connected, this edge is redundant
            if (unionNodes(edge[0], edge[1], parent, rank)) {
                return edge;
            }
        }

        return {}; // If no redundant connection is found
    }

private:
    int find(int node, std::vector<int>& parent) {
        // Path compression optimization
        if (parent[node] != node) {
            parent[node] = find(parent[node], parent);
        }
        return parent[node];
    }

    bool unionNodes(int u, int v, std::vector<int>& parent, std::vector<int>& rank) {
        int rootU = find(u, parent);
        int rootV = find(v, parent);

        if (rootU == rootV) {
            return true; // u and v are already connected
        }

        // Union by rank
        if (rank[rootU] > rank[rootV]) {
            parent[rootV] = rootU;
        } else if (rank[rootU] < rank[rootV]) {
            parent[rootU] = rootV;
        } else {
            parent[rootV] = rootU;
            rank[rootU]++;
        }
        return false;
    }
};
```

## Approach 2: DFS to Detect Cycle

### Solution
```cpp
// Time Complexity: O(n^2), in the worst case for a dense graph
// Space Complexity: O(n)
#include <vector>
#include <list>

class Solution {
public:
    std::vector<int> findRedundantConnection(std::vector<std::vector<int>>& edges) {
        int n = edges.size();
        std::vector<std::list<int>> graph(n + 1);

        for (const auto& edge : edges) {
            if (hasCycleDFS(graph, edge[0], edge[1], std::vector<bool>(n + 1, false))) {
                return edge;
            }
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }

        return {}; // If no redundant connection is found
    }

private:
    bool hasCycleDFS(std::vector<std::list<int>>& graph, int source, int target, std::vector<bool>& visited) {
        if (visited[source]) return false;

        visited[source] = true;

        for (const int neighbor : graph[source]) {
            if (neighbor == target) continue;
            if (visited[neighbor] || hasCycleDFS(graph, neighbor, target, visited)) {
                return true;
            }
        }

        return false;
    }
};
```

