# [Leetcode 684: Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approaches

1. [Union-Find Algorithm (Disjoint Set)](#union-find-algorithm-disjoint-set)

---

### Union-Find Algorithm (Disjoint Set)

The "Redundant Connection" problem requires us to find an edge in a graph that, when removed, makes the graph a tree. In a tree with `n` nodes, there are exactly `n-1` edges. Thus, if a graph has `n` nodes with `n` edges, there must be exactly one cycle, and the task is to find the edge that completes this cycle.

The simplest method to address this problem uses a Union-Find algorithm, also known as Disjoint Set Union (DSU). The intuition is that we can keep track of connected components and detect when adding an edge creates a cycle:

- **Union-Find Algorithm**: This data structure supports two operations efficiently: 
  - **Find**: Determine the root/parent of a node.
  - **Union**: Connect two nodes. Detect if they are already connected (in the same component).

By performing these operations while iterating through the edges, we can determine if adding a new edge leads to a cycle (i.e., both nodes in the edge are already connected).

#### Steps/Algorithm:

1. Initialize a root array to keep track of the parent node for each node. Initially, each node is its own parent.
2. For each edge `(u, v)`:
   - Use the **Find** operation to determine the roots of `u` and `v`.
   - If the roots are the same, return `[u, v]`, as this edge creates a cycle.
   - Otherwise, use the **Union** operation to merge the roots of `u` and `v`.

The implementation of Find and Union operations is optimized using path compression and union by rank/size to achieve nearly constant time complexity for each operation.

```csharp
public class Solution {
    public int[] FindRedundantConnection(int[][] edges) {
        // Initialize a parent array to keep track of the connected components
        int[] parent = new int[edges.Length + 1];
        
        // Initially, every node is its own parent
        for (int i = 0; i < parent.Length; i++) {
            parent[i] = i;
        }
        
        foreach (var edge in edges) {
            int u = edge[0];
            int v = edge[1];

            // Find the root ancestors of both nodes
            int rootU = Find(parent, u);
            int rootV = Find(parent, v);

            // If both nodes have the same root, this edge is redundant
            if (rootU == rootV) {
                return edge;
            }
            
            // Union the components by connecting the roots
            Union(parent, rootU, rootV);
        }
        
        // If no redundant edge is found (though there should be exactly one), return an empty array
        return new int[] {};
    }
    
    // Find with path compression
    private int Find(int[] parent, int node) {
        // If the node is its own parent, it's the root
        if (parent[node] != node) {
            // Recursively find the parent and compress the path
            parent[node] = Find(parent, parent[node]);
        }
        return parent[node];
    }
    
    // Union by connecting the roots
    private void Union(int[] parent, int rootU, int rootV) {
        parent[rootU] = rootV; // Merge by making rootV the root of rootU
    }
}
```

**Time Complexity**: \(O(N)\) - where \(N\) is the number of edges, due to the nearly constant time for find/union operations using path compression.

**Space Complexity**: \(O(N)\) - for storing the parent for each node.

