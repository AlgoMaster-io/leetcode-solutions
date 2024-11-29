# 684. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approach 1: Union-Find Algorithm

### Solution
```java
// Time Complexity: O(n * α(n)), where α is the inverse Ackermann function
// Space Complexity: O(n)
public class Solution {
    
    public int[] findRedundantConnection(int[][] edges) {
        int n = edges.length;
        int[] parent = new int[n + 1];
        int[] rank = new int[n + 1];

        // Initialize the parent for each node as itself
        for (int i = 1; i <= n; i++) {
            parent[i] = i;
            rank[i] = 1; // Initialize the rank
        }
        
        for (int[] edge : edges) {
            // If the two nodes are already connected, this edge is redundant
            if (union(edge[0], edge[1], parent, rank)) {
                return edge;
            }
        }
        
        return new int[0]; // If no redundant connection is found
    }

    private int find(int node, int[] parent) {
        // Path compression optimization
        if (parent[node] != node) {
            parent[node] = find(parent[node], parent);
        }
        return parent[node];
    }

    private boolean union(int u, int v, int[] parent, int[] rank) {
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
}
```

## Approach 2: DFS to Detect Cycle

### Solution
```java
// Time Complexity: O(n^2), in the worst case for a dense graph
// Space Complexity: O(n)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    
    public int[] findRedundantConnection(int[][] edges) {
        int n = edges.length;
        List<Integer>[] graph = new List[n + 1];
        
        // Initialize adjacency list
        for (int i = 0; i <= n; i++) {
            graph[i] = new ArrayList<>();
        }
        
        for (int[] edge : edges) {
            if (hasCycleDFS(graph, edge[0], edge[1], new boolean[n + 1])) {
                return edge;
            }
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        
        return new int[0]; // If no redundant connection is found
    }

    private boolean hasCycleDFS(List<Integer>[] graph, int source, int target, boolean[] visited) {
        if (visited[source]) return false;
        
        visited[source] = true;
        
        for (int neighbor : graph[source]) {
            if (neighbor == target) continue;
            if (visited[neighbor] || hasCycleDFS(graph, neighbor, target, visited)) {
                return true;
            }
        }
        
        return false;
    }
}
```


