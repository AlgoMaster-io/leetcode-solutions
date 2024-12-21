# [Leetcode 834: Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approaches
- [Approach 1: Brute Force DFS](#approach-1-brute-force-dfs)
- [Approach 2: Optimized DFS with Precomputation and Post-processing](#approach-2-optimized-dfs-with-precomputation-and-post-processing)

---

## Approach 1: Brute Force DFS

### Intuition
A naive approach is to calculate the sum of distances from every node to every other node. For each node as the root, we do a DFS to compute its distance to all other nodes. This results in a straightforward but inefficient solution where we calculate these sums individually for each node.

### Algorithm
1. For each node, perform a DFS to compute the distances to all other nodes.
2. Accumulate these distances to get the sum for each node.

### Code
```cpp
#include <vector>
#include <iostream>
using namespace std;

void dfs(int node, int parent, vector<vector<int>> &graph, vector<int> &distance, vector<bool> &visited, int depth) {
    distance[node] += depth;
    visited[node] = true;
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor, node, graph, distance, visited, depth + 1);
        }
    }
}

vector<int> sumOfDistancesInTree(int N, vector<vector<int>>& edges) {
    vector<vector<int>> graph(N);
    for (const auto& edge : edges) {
        graph[edge[0]].push_back(edge[1]);
        graph[edge[1]].push_back(edge[0]);
    }
    
    vector<int> result(N, 0);
    for (int i = 0; i < N; ++i) {
        vector<int> distance(N, 0);
        vector<bool> visited(N, false);
        dfs(i, -1, graph, distance, visited, 0);
        result[i] = distance[i];
    }
    return result;
}

int main() {
    vector<vector<int>> edges = {{0,1}, {0,2}, {2,3}, {2,4}, {2,5}};
    int N = 6;
    vector<int> result = sumOfDistancesInTree(N, edges);
    for (int dist : result) {
        cout << dist << " ";
    }
    return 0;
}
```

### Complexity
- **Time Complexity:** \(O(N^2)\) because for each node, a full traversal of the tree is done.
- **Space Complexity:** \(O(N)\) for storing the graph and auxiliary computations.

---

## Approach 2: Optimized DFS with Precomputation and Post-processing

### Intuition
Instead of recalculating the sum of distances repeatedly, we can utilize a more efficient DFS strategy that calculates these sums more incrementally and efficiently. The idea is to compute this in two DFS passes:
1. First Pass: Use DFS to compute the sum of distances for an arbitrary root node (e.g., node 0) and count the number of nodes in the subtree rooted at each node.
2. Second Pass: Another DFS to utilize the computed subtree information to calculate the results for all other nodes using the result from the root node.

### Algorithm
1. Perform a DFS to compute the initial distances and subtree sizes.
2. Use these computations in a second DFS to efficiently calculate distances for all nodes without redundant work.

### Code
```cpp
#include <vector>
#include <iostream>
using namespace std;

void dfs1(int node, int parent, vector<vector<int>> &graph, vector<int> &count, vector<int> &dist) {
    for (int neighbor : graph[node]) {
        if (neighbor != parent) {
            dfs1(neighbor, node, graph, count, dist);
            count[node] += count[neighbor];
            dist[node] += dist[neighbor] + count[neighbor]; // distance to all nodes in the subtree
        }
    }
    count[node] += 1; // count the node itself
}

void dfs2(int node, int parent, vector<vector<int>> &graph, vector<int> &count, vector<int> &dist, vector<int> &result, int N) {
    result[node] = dist[node];
    
    for (int neighbor : graph[node]) {
        if (neighbor != parent) {
            // Update for the new root
            int originalDistNode = dist[node];
            int originalDistNeighbor = dist[neighbor];
            int originalCountNeighbor = count[neighbor];
            
            dist[node] -= dist[neighbor] + count[neighbor];
            count[node] -= count[neighbor];
            dist[neighbor] += dist[node] + count[node];
            count[neighbor] += count[node];
            
            dfs2(neighbor, node, graph, count, dist, result, N);
            
            // Restore original values
            count[neighbor] = originalCountNeighbor;
            dist[neighbor] = originalDistNeighbor;
            count[node] += originalCountNeighbor;
            dist[node] = originalDistNode;
        }
    }
}

vector<int> sumOfDistancesInTree(int N, vector<vector<int>>& edges) {
    vector<vector<int>> graph(N);
    for (const auto& edge : edges) {
        graph[edge[0]].push_back(edge[1]);
        graph[edge[1]].push_back(edge[0]);
    }
    
    vector<int> result(N, 0);
    vector<int> count(N, 0); // size of subtree rooted at each node
    vector<int> dist(N, 0);  // sum of distances from node 0
    
    dfs1(0, -1, graph, count, dist);
    dfs2(0, -1, graph, count, dist, result, N);
    
    return result;
}

int main() {
    vector<vector<int>> edges = {{0,1}, {0,2}, {2,3}, {2,4}, {2,5}};
    int N = 6;
    vector<int> result = sumOfDistancesInTree(N, edges);
    for (int dist : result) {
        cout << dist << " ";
    }
    return 0;
}
```

### Complexity
- **Time Complexity:** \(O(N)\), DFS runs linear to the number of nodes and edges.
- **Space Complexity:** \(O(N)\), mainly for storing graph, subtree sizes, and auxiliary structures necessary for DFS.


