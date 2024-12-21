# [Leetcode 310: Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

## Approaches
- [Approach 1: Naive Approach using DFS](#approach-1-naive-approach-using-dfs)
- [Approach 2: Topological Sorting using BFS](#approach-2-topological-sorting-using-bfs)

### Approach 1: Naive Approach using DFS

#### Intuition
The problem asks us to find all the roots that would produce the minimum height trees. One of the naive approaches is to consider every node as a potential root and compute the height of the resulting tree. The minimum height trees would be those where the height is minimum for all considered roots.

1. For each node, treat it as the root.
2. Use Depth First Search (DFS) to calculate the height of the tree with that node as the root.
3. Keep track of the minimum height observed and the nodes that produce this height.

#### Code
```typescript
function findMinHeightTrees(n: number, edges: number[][]): number[] {
    if (n === 1) return [0];

    const adjacencyList = Array.from({length: n}, () => []);
    for (let [u, v] of edges) {
        adjacencyList[u].push(v);
        adjacencyList[v].push(u);
    }

    let minHeight = Infinity;
    let result: number[] = [];
    
    function dfs(node: number, parent: number): number {
        let height = 0;
        for (let neighbor of adjacencyList[node]) {
            if (neighbor !== parent) {
                height = Math.max(height, dfs(neighbor, node) + 1);
            }
        }
        return height;
    }
    
    for (let i = 0; i < n; i++) {
        const height = dfs(i, -1);
        if (height < minHeight) {
            minHeight = height;
            result = [i];
        } else if (height === minHeight) {
            result.push(i);
        }
    }

    return result;
}
```

#### Time Complexity
- **O(N^2)**: For each node treated as root, we perform an `O(N)` DFS traversal.

#### Space Complexity
- **O(N)**: Space used by the adjacency list and recursive call stack.

### Approach 2: Topological Sorting using BFS

#### Intuition
Instead of calculating the height for each node, we can take advantage of the inherent properties of trees:
- A tree with N nodes has exactly N-1 edges.
- We can progressively remove leaves (nodes with only one connection) until we reach the centroid(s) of the tree. The centroid of a tree is most centrally located and gives minimum height when used as root.

1. Initialize a queue with all leaf nodes (nodes with only one edge).
2. Continuously remove leaves, updating the degree (number of connections) of their neighbors.
3. Add new leaves to the queue.
4. The last remaining nodes in the queue are the roots of the Minimum Height Trees.

#### Code
```typescript
function findMinHeightTrees(n: number, edges: number[][]): number[] {
    if (n === 1) return [0];

    const adjacencyList = Array.from({length: n}, () => []);
    const degree = Array(n).fill(0);

    for (let [u, v] of edges) {
        adjacencyList[u].push(v);
        adjacencyList[v].push(u);
        degree[u]++;
        degree[v]++;
    }

    let leaves: number[] = [];
    for (let i = 0; i < n; i++) {
        if (degree[i] === 1) {
            leaves.push(i);
        }
    }

    let remainingNodes = n;
    while (remainingNodes > 2) {
        remainingNodes -= leaves.length;
        const newLeaves: number[] = [];
        for (let leaf of leaves) {
            for (let neighbor of adjacencyList[leaf]) {
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

#### Time Complexity
- **O(N)**: Each node is processed once, each edge is considered once.

#### Space Complexity
- **O(N)**: Space used by the adjacency list and degree array.

