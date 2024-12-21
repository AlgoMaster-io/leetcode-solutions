[Leetcode 785: Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)

## Approaches

1. [DFS Approach](#dfs-approach)
2. [BFS Approach](#bfs-approach)
3. [Union-Find Approach](#union-find-approach)

### DFS Approach

#### Intuition
The idea is to use DFS to try to color the graph using two colors in such a way that no two adjacent nodes have the same color. If we can successfully paint the entire graph in two colors, the graph is bipartite.

#### Code
```csharp
public class Solution {
    public bool IsBipartite(int[][] graph) {
        int n = graph.Length;
        int[] colors = new int[n]; // Array to track colors of the nodes (0 means uncolored)

        for (int i = 0; i < n; i++) {
            if (colors[i] == 0 && !DFS(i, 1, colors, graph)) { 
                return false; // If any component cannot be colored bipartitely, return false
            }
        }
        return true;
    }

    private bool DFS(int node, int color, int[] colors, int[][] graph) {
        colors[node] = color; // Color the current node

        foreach (int neighbor in graph[node]) {
            if (colors[neighbor] == color) {
                return false; // If neighbor has the same color, graph is not bipartite
            }
            if (colors[neighbor] == 0 && !DFS(neighbor, -color, colors, graph)) {
                return false; // Paint with opposite color and check
            }
        }
        return true;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(V + E)\), where \(V\) is the number of vertices and \(E\) is the number of edges. We visit each node and edge once.
- **Space Complexity:** \(O(V)\) for the color array and recursive call stack in the worst case.

### BFS Approach

#### Intuition
Similar to DFS, but we use BFS to traverse the nodes level by level, alternating colors as we move.

#### Code
```csharp
public class Solution {
    public bool IsBipartite(int[][] graph) {
        int n = graph.Length;
        int[] colors = new int[n];

        for (int i = 0; i < n; i++) {
            if (colors[i] != 0) continue; // Skip already colored nodes

            Queue<int> queue = new Queue<int>();
            queue.Enqueue(i);
            colors[i] = 1; // Start coloring this component with color 1

            while (queue.Count > 0) {
                int node = queue.Dequeue();

                foreach (int neighbor in graph[node]) {
                    if (colors[neighbor] == 0) { // If the neighbor is uncolored, color with the opposite color
                        colors[neighbor] = -colors[node];
                        queue.Enqueue(neighbor);
                    } else if (colors[neighbor] == colors[node]) {
                        return false; // If a neighbor has the same color, the graph is not bipartite
                    }
                }
            }
        }
        return true;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(V + E)\) for similar reasons as the DFS approach.
- **Space Complexity:** \(O(V)\) for the color array and queue storage in the worst case.

### Union-Find Approach

#### Intuition
In a bipartite graph, if node x and y are in the same set, then they cannot be directly connected. We use the union-find data structure to maintain and check the connectivity.

#### Code
```csharp
public class Solution {
    private int[] parent;

    public bool IsBipartite(int[][] graph) {
        int n = graph.Length;
        parent = new int[n];
        
        for (int i = 0; i < n; ++i) {
            parent[i] = i; // Initially, every node is its own parent
        }

        for (int u = 0; u < n; u++) {
            for (int v = 1; v < graph[u].Length; ++v) {
                int firstNeighbor = graph[u][0];
                int currentNeighbor = graph[u][v];

                if (Find(u) == Find(currentNeighbor)) {
                    return false; // If the node and neighbor have the same root, then the graph is not bipartite
                }

                Union(firstNeighbor, currentNeighbor); // Union the current pair
            }
        }
        return true;
    }

    private int Find(int node) {
        if (parent[node] != node) {
            parent[node] = Find(parent[node]); // Path compression
        }
        return parent[node];
    }

    private void Union(int node1, int node2) {
        parent[Find(node1)] = Find(node2); // Union by rank not needed due to low constraint graph size
    }
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(V + E \cdot \alpha(V))\), where \(\alpha\) is the inverse Ackermann function, which grows very slowly. \(V\) being the number of vertices and \(E\) edges.
- **Space Complexity:** \(O(V)\), for storing the parent array.

