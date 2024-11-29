# 684. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approach 1: Union-Find Algorithm

### Solution
```csharp
// Time Complexity: O(n * α(n)), where α is the inverse Ackermann function
// Space Complexity: O(n)
public class Solution {
    
    public int[] FindRedundantConnection(int[][] edges) {
        int n = edges.Length;
        int[] parent = new int[n + 1];
        int[] rank = new int[n + 1];

        // Initialize the parent for each node as itself
        for (int i = 1; i <= n; i++) {
            parent[i] = i;
            rank[i] = 1; // Initialize the rank
        }
        
        foreach (var edge in edges) {
            // If the two nodes are already connected, this edge is redundant
            if (Union(edge[0], edge[1], parent, rank)) {
                return edge;
            }
        }
        
        return new int[0]; // If no redundant connection is found
    }

    private int Find(int node, int[] parent) {
        // Path compression optimization
        if (parent[node] != node) {
            parent[node] = Find(parent[node], parent);
        }
        return parent[node];
    }

    private bool Union(int u, int v, int[] parent, int[] rank) {
        int rootU = Find(u, parent);
        int rootV = Find(v, parent);
        
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
}
```

## Approach 2: DFS to Detect Cycle

### Solution
```csharp
// Time Complexity: O(n^2), in the worst case for a dense graph
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    
    public int[] FindRedundantConnection(int[][] edges) {
        int n = edges.Length;
        List<int>[] graph = new List<int>[n + 1];
        
        // Initialize adjacency list
        for (int i = 0; i <= n; i++) {
            graph[i] = new List<int>();
        }
        
        foreach (var edge in edges) {
            if (HasCycleDFS(graph, edge[0], edge[1], new bool[n + 1])) {
                return edge;
            }
            graph[edge[0]].Add(edge[1]);
            graph[edge[1]].Add(edge[0]);
        }
        
        return new int[0]; // If no redundant connection is found
    }

    private bool HasCycleDFS(List<int>[] graph, int source, int target, bool[] visited) {
        if (visited[source]) return false;
        
        visited[source] = true;
        
        foreach (var neighbor in graph[source]) {
            if (neighbor == target) continue;
            if (visited[neighbor] || HasCycleDFS(graph, neighbor, target, visited)) {
                return true;
            }
        }
        
        return false;
    }
}
```

