## [Leetcode 684: Redundant Connection](https://leetcode.com/problems/redundant-connection/)

- [Approach 1: Basic Union-Find](#approach-1-basic-union-find)
- [Approach 2: Optimized Union-Find with Path Compression](#approach-2-optimized-union-find-with-path-compression)

---

### Approach 1: Basic Union-Find

**Intuition**

The problem can be tackled by using the Union-Find (or Disjoint Set Union) data structure. The objective is to find an edge that, if removed, leaves the graph as a tree which implies that all nodes are connected without any cycles. Let's break down how Union-Find can help:

1. **Union-Find Basics**: In this structure, each node initially is its own parent. We find the root or parent of any node using the `find` function.
2. **Union Operation**: We "union" two nodes by making the root of one node point to the root of another.
3. **Cycle Detection**: A cycle is found if we try to union two nodes that share the same root.

The basic idea here is to iterate over all edges, and for each edge, check if both vertices have the same root; if they do, it indicates a cycle. If not, union the two vertices.

**Code**

```javascript
function findRedundantConnection(edges) {
    const parent = Array.from({ length: edges.length + 1 }, (_, index) => index);

    // Find the root/parent of a node
    const find = (node) => {
        while (node !== parent[node]) {
            node = parent[node];
        }
        return node;
    };

    // Union two nodes
    const union = (node1, node2) => {
        const root1 = find(node1);
        const root2 = find(node2);
        if (root1 === root2) return false;
        parent[root1] = root2;
        return true;
    };

    for (const [u, v] of edges) {
        if (!union(u, v)) return [u, v];
    }
    return [];
}
```

**Time Complexity**: O(n) where n is the number of edges. Each union-find operation is proportional to the number of edges.

**Space Complexity**: O(n) for the parent array.

### Approach 2: Optimized Union-Find with Path Compression

**Intuition**

This approach optimizes the basic union-find by using "path compression". Path compression helps make the find operation faster by making nodes point directly to the root of the set, minimizing the path length whenever possible.

1. **Path Compression**: In the `find` operation, we recursively find the root and make each node point directly to the root.
2. **Union by Rank (can be added further for even better optimization)**: Typically used with path compression, it involves attaching the smaller tree to the root of the larger tree. We won't cover this here as path compression is often sufficient for competitive problems.

**Code**

```javascript
function findRedundantConnection(edges) {
    const parent = Array.from({ length: edges.length + 1 }, (_, index) => index);

    // Optimized find function with path compression
    const find = (node) => {
        if (parent[node] !== node) {
            parent[node] = find(parent[node]); // Path compression
        }
        return parent[node];
    };

    const union = (node1, node2) => {
        const root1 = find(node1);
        const root2 = find(node2);
        if (root1 === root2) return false;
        parent[root1] = root2;
        return true;
    };

    for (const [u, v] of edges) {
        if (!union(u, v)) return [u, v];
    }
    return [];
}
```

**Time Complexity**: O(n * α(n)), where α(n) is the inverse Ackermann function, which grows very slowly, making it nearly constant time.

**Space Complexity**: O(n) for the parent array.

