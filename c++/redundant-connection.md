# [684. Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approaches
- [Union-Find (Disjoint Set Union)](#Union-Find-DSU)
- [Union-Find with Path Compression and Union by Rank](#Union-Find-with-Path-Compression-and-Union-by-Rank)

---

## Union-Find (Disjoint Set Union)

### Intuition
The problem of finding a redundant connection in a graph translates to detecting a cycle within the graph. Using the Union-Find algorithm is suitable for this task because it effectively helps to determine whether adding an edge would form a loop. The idea is simple: for each edge, check if the two connecting nodes are already in the same connected component. If they are, this edge would form a cycle, and hence, it is redundant.

### Approach
1. Initialize a `parent` array where each node is its own parent initially.
2. Traverse each edge and use a "find" operation to determine the root parents of both endpoints of the edge.
3. If both endpoints have the same root parent, a cycle is detected, and hence, the edge is redundant.
4. If not, perform a "union" operation to connect the two components by making one the parent of the other.

### Implementation

```cpp
class Solution {
public:
    int find(vector<int>& parent, int x) {
        if (parent[x] != x) {
            return find(parent, parent[x]);
        }
        return parent[x];
    }

    void unionSets(vector<int>& parent, int x, int y) {
        int rootX = find(parent, x);
        int rootY = find(parent, y);
        if (rootX != rootY) {
            parent[rootX] = rootY;
        }
    }
    
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> parent(n + 1);

        // Initially, each node is its own parent
        for (int i = 1; i <= n; i++) {
            parent[i] = i;
        }
        
        for (const auto& edge : edges) {
            // Find root parents of the two vertices of the edge
            int rootX = find(parent, edge[0]);
            int rootY = find(parent, edge[1]);

            // If they are already connected, we found a redundant connection
            if (rootX == rootY) {
                return edge;
            } else {
                // Union the sets if not already connected
                unionSets(parent, rootX, rootY);
            }
        }

        return {};
    }
};
```

### Time Complexity
- **Time:** O(n * α(n)) due to the inverse Ackermann function involved with find operations.
- **Space:** O(n) due to the `parent` array.

---

## Union-Find with Path Compression and Union by Rank

### Intuition
While the basic Union-Find approach works, we can enhance the performance further using two techniques:
- **Path Compression:** This technique speeds up the "find" operation by making nodes point directly to the root of the set, thus flattening the tree structure.
- **Union by Rank:** Improvement for the "union" operation that ensures the smaller tree points to the root of the larger tree, maintaining a balanced tree structure.

### Approach
The approach largely mirrors the basic Union-Find algorithm except that:
1. During the "find" operation, path compression ensures all nodes on the path point directly to the root.
2. During the "union" operation, we use rank to attach the smaller tree under the root of the bigger tree.

### Implementation

```cpp
class Solution {
public:
    int find(vector<int>& parent, int x) {
        if (parent[x] != x) {
            parent[x] = find(parent, parent[x]); // Path compression
        }
        return parent[x];
    }

    void unionSets(vector<int>& parent, vector<int>& rank, int x, int y) {
        int rootX = find(parent, x);
        int rootY = find(parent, y);

        // Union by rank
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
        }
    }
    
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> parent(n + 1);
        vector<int> rank(n + 1, 0);

        // Initially, each node is its own parent
        for (int i = 1; i <= n; i++) {
            parent[i] = i;
        }
        
        for (const auto& edge : edges) {
            int rootX = find(parent, edge[0]);
            int rootY = find(parent, edge[1]);

            if (rootX == rootY) {
                return edge;
            } else {
                unionSets(parent, rank, rootX, rootY);
            }
        }

        return {};
    }
};
```

### Time Complexity
- **Time:** O(n * α(n)) where α is the inverse Ackermann function, which is very small and nearly constant time.
- **Space:** O(n) owing to storage for the `parent` and `rank` arrays.

