# [Leetcode 310: Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

Solutions:
- [Approach 1: Brute Force Approach (DFS)](#approach-1-brute-force-approach-dfs)
- [Approach 2: Topological Sort with BFS (Optimal)](#approach-2-topological-sort-with-bfs-optimal)

## Approach 1: Brute Force Approach (DFS)

### Intuition

The brute force way to solve the Minimum Height Trees problem is as follows:

1. For each node, consider it as the root of the tree.
2. Calculate the height of the tree by doing a Depth First Search (DFS) from this root.
3. Keep track of the minimum height and its corresponding root.

This approach leverages the basic property of trees by treating each node as a potential root and evaluating the maximum distance (tree height) it holds when it's a root.

### Code

```javascript
function findMinHeightTrees(n, edges) {
    if (n === 1) {
        return [0]; // Only one node, it's the root and minimum height tree
    }

    // Construct the adjacency list to represent the graph
    const adjList = Array.from({ length: n }, () => []);
    for (let [u, v] of edges) {
        adjList[u].push(v);
        adjList[v].push(u);
    }

    // Function to perform DFS and return depth
    function dfs(node, visited) {
        visited[node] = true;
        let maxDepth = 0;

        for (let neighbor of adjList[node]) {
            if (!visited[neighbor]) {
                maxDepth = Math.max(maxDepth, dfs(neighbor, visited));
            }
        }

        visited[node] = false; // Mark as unvisited for other roots
        return maxDepth + 1;
    }

    let minTreeHeight = Infinity;
    let result = [];

    // Evaluate height for each node being the root
    for (let i = 0; i < n; i++) {
        const visited = Array(n).fill(false);
        const treeHeight = dfs(i, visited);
        if (treeHeight < minTreeHeight) {
            minTreeHeight = treeHeight;
            result = [i]; // Reset the result; new minimum found
        } else if (treeHeight === minTreeHeight) {
            result.push(i); // Another root with the same minimum height
        }
    }

    return result;
}
```

### Complexity Analysis

- **Time Complexity:** O(n^2), where `n` is the number of nodes. For each node, DFS could potentially explore all nodes.
- **Space Complexity:** O(n^2) for the adjacency list and recursion stack.

## Approach 2: Topological Sort with BFS (Optimal)

### Intuition

The optimal approach to solving this problem is by using a Topological Sort with Breadth First Search (BFS). This method is akin to peeling an onion layer-by-layer. Here’s how it works:

1. A tree with `n` nodes has exactly `n-1` edges. A leaf node is one that has only one connection (edge).
2. Using topological sort, nodes are removed level by level. We start with leaf nodes, reducing the complexity since these can never be roots of the Minimum Height Trees.
3. The idea is to continuously remove leaf nodes until two or fewer nodes are left. These nodes are the roots of the Minimum Height Trees.

### Code

```javascript
function findMinHeightTrees(n, edges) {
    if (n === 1) {
        return [0]; // Special case, only one node
    }

    // Construct the adjacency list
    const adjList = Array.from({ length: n }, () => []);
    const degree = Array(n).fill(0);

    for (let [u, v] of edges) {
        adjList[u].push(v);
        adjList[v].push(u);
        degree[u]++;
        degree[v]++;
    }

    // Find all leaves
    let leaves = [];
    for (let i = 0; i < n; i++) {
        if (degree[i] === 1) {
            leaves.push(i);
        }
    }

    // Remove leaves iteratively
    let remainingNodes = n;
    while (remainingNodes > 2) {
        remainingNodes -= leaves.length;
        const newLeaves = [];
        
        for (const leaf of leaves) {
            for (const neighbor of adjList[leaf]) {
                degree[neighbor]--;
                if (degree[neighbor] === 1) {
                    newLeaves.push(neighbor);
                }
            }
        }
        leaves = newLeaves;
    }

    return leaves;
}
```

### Complexity Analysis

- **Time Complexity:** O(n), since each node and each edge will be considered only once.
- **Space Complexity:** O(n), due to the adjacency list and degree array used.
  
In this way, Approach 2 provides a more optimal and efficient solution to the Minimum Height Trees problem, by systematically reducing complexities step-by-step.

