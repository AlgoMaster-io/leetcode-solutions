# [Leetcode 310: Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

## Solutions

- [Solution 1: Basic Breadth-First Search (BFS) Approach](#solution-1-basic-breadth-first-search-bfs-approach)
- [Solution 2: Optimal Topological Sorting Approach with BFS](#solution-2-optimal-topological-sorting-approach-with-bfs)

### Solution 1: Basic Breadth-First Search (BFS) Approach

#### Intuition
The problem requires finding the root nodes which minimize the height of the tree. A naive approach is to construct the tree for each node and calculate the maximum depth from that node repeatedly, which however, is computationally expensive.

This approach involves trimming leaves level by level using BFS, which is similar to peeling an onion layer by layer until we reach the core. The nodes left when the number of non-leaf nodes is less than or equal to two will be the roots of the minimum height trees.

#### Approach
1. Start by identifying all leaf nodes (nodes with only one connection).
2. Remove these leaf nodes, reducing the graph into a smaller problem.
3. Repeat this process, which will eventually lead down to 1 or 2 nodes.
4. These nodes will represent the centroids - the roots of a Minimum Height Tree (MHT).

#### Time Complexity
The algorithm runs in O(n), as each edge and node is visited once.

#### Space Complexity
The space complexity is O(n) due to the adjacency list and other structures used.

```cpp
#include <vector>
#include <queue>
#include <unordered_set>

using namespace std;

vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
    if (n == 1) return {0};  // Edge-case: single node tree.

    // Create an adjacency list for the graph.
    vector<unordered_set<int>> adj(n);
    for (auto edge : edges) {
        adj[edge[0]].insert(edge[1]);
        adj[edge[1]].insert(edge[0]);
    }

    // Initialize a queue with all the leaf nodes.
    queue<int> leaves;
    for (int i = 0; i < n; i++) {
        if (adj[i].size() == 1) leaves.push(i);
    }

    // Iteratively remove leaves layer-by-layer until we reach the core.
    while (n > 2) {
        int leavesSize = leaves.size();
        n -= leavesSize;  // Updating the count of remaining nodes.
        
        // Remove each leaf node.
        for (int i = 0; i < leavesSize; i++) {
            int leaf = leaves.front();
            leaves.pop();
            
            for (int neighbor : adj[leaf]) {
                adj[neighbor].erase(leaf);  // Remove the connection to the leaf.
                if (adj[neighbor].size() == 1) leaves.push(neighbor);  // Identify new leaves.
            }
        }
    }
    
    // The remaining nodes are the centroids.
    vector<int> mhtRoots;
    while (!leaves.empty()) {
        mhtRoots.push_back(leaves.front());
        leaves.pop();
    }
    
    return mhtRoots;
}
```

### Solution 2: Optimal Topological Sorting Approach with BFS

#### Intuition
This solution uses a more systematic approach leveraging topological sorting concepts, akin to finding the center directly. It's a polished method that systematically prunes the tree to identify the absolute centroid or couple of centroids.

#### Approach
1. Build the graph in an adjacency list format.
2. Initialize an array of indegrees to keep track of the degree of each node.
3. Starting with all leaves (nodes with an indegree of 1), perform a topological sort by removing these leaves iteratively.
4. As we remove leaves, update the indegrees, and when only 1 or 2 nodes remain, these are our results as they will be the core of the graph closest to the center (MHT roots).

#### Time Complexity
This approach also operates in O(n), with each node processed once.

#### Space Complexity
Similar to the naive BFS approach, this uses O(n) auxiliary space.

```cpp
#include <vector>
#include <queue>

using namespace std;

vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
    if (n == 1) return {0};  // Single-node special case.

    // Construct adjacency list and indegree array.
    vector<vector<int>> adj(n);
    vector<int> degree(n, 0);
    for (const auto& edge : edges) {
        adj[edge[0]].push_back(edge[1]);
        adj[edge[1]].push_back(edge[0]);
        degree[edge[0]]++;
        degree[edge[1]]++;
    }

    // Initial queue populate with leaf nodes (degree 1).
    queue<int> leaves;
    for (int i = 0; i < n; ++i) {
        if (degree[i] == 1) {
            leaves.push(i);
        }
    }

    // Remove leaves layer by layer until we reach the center.
    int remainingNodes = n;
    while (remainingNodes > 2) {
        int leavesSize = leaves.size();
        remainingNodes -= leavesSize;
        for (int i = 0; i < leavesSize; ++i) {
            int leaf = leaves.front();
            leaves.pop();
            // For all neighbors, reduce their degree due to removing this leaf.
            for (int neighbor : adj[leaf]) {
                degree[neighbor]--;
                if (degree[neighbor] == 1) {
                    leaves.push(neighbor);
                }
            }
        }
    }

    // Capture the remaining 1 or 2 nodes.
    vector<int> mhtRoots;
    while (!leaves.empty()) {
        mhtRoots.push_back(leaves.front());
        leaves.pop();
    }

    return mhtRoots;
}
```
Both solutions approach the problem efficiently by systematically reducing the tree until reaching the centrals, i.e., minimizing the tree height in potential configurations. This problem embraces the beauty of graph theory and BFS to arrive at a solution suitable for tree-like structures.

