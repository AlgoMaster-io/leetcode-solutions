# [Leetcode 863: All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

## Approaches
1. [Breadth-First Search (BFS) for Graph Conversion](#approach-1-breadth-first-search-bfs-for-graph-conversion)
2. [Depth-First Search (DFS) Direct Traversal](#approach-2-depth-first-search-dfs-direct-traversal)

### Approach 1: Breadth-First Search (BFS) for Graph Conversion

#### Intuition
Convert the binary tree into an undirected graph. Then, perform a BFS starting from the target node to find all nodes that are distance 'K' from it. The key idea is to treat the problem as finding nodes at a specific distance in a graph once we convert the tree to a graph.

#### Steps:
1. Use recursive DFS to convert the binary tree into an undirected graph by storing connections in an adjacency list.
2. Start BFS from the target node and explore all nodes reachable via BFS.
3. Continue the BFS until you reach the required distance K.

#### Code:
```typescript
class TreeNode {
  val: number;
  left: TreeNode | null;
  right: TreeNode | null;
  constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
    this.val = val ?? 0;
    this.left = left ?? null;
    this.right = right ?? null;
  }
}

function distanceK(root: TreeNode | null, target: TreeNode | null, K: number): number[] {
  const graph: Map<number, number[]> = new Map();

  function buildGraph(node: TreeNode | null, parent: TreeNode | null) {
    if (!node) return;

    if (parent) {
      if (!graph.has(node.val)) graph.set(node.val, []);
      if (!graph.has(parent.val)) graph.set(parent.val, []);
      graph.get(node.val)!.push(parent.val);
      graph.get(parent.val)!.push(node.val);
    }

    buildGraph(node.left, node);
    buildGraph(node.right, node);
  }

  // Build the graph from the tree
  buildGraph(root, null);

  const result: number[] = [];
  const visited: Set<number> = new Set();

  function bfs(start: number, distance: number) {
    let queue: number[] = [start];
    visited.add(start);

    while (queue.length) {
      if (distance === 0) {
        result.push(...queue);
        return;
      }

      const nextQueue: number[] = [];
      for (let node of queue) {
        for (let neighbor of graph.get(node) || []) {
          if (!visited.has(neighbor)) {
            visited.add(neighbor);
            nextQueue.push(neighbor);
          }
        }
      }
      queue = nextQueue;
      distance--;
    }
  }

  // Execute BFS from the target node
  if (target) {
    bfs(target.val, K);
  }

  return result;
}
```

#### Complexity:
- **Time Complexity**: O(N), where N is the number of nodes in the tree. We need to visit each node twice (once to create graph and once for BFS).
- **Space Complexity**: O(N), due to the storage in the adjacency list and the auxiliary queue used in BFS.

### Approach 2: Depth-First Search (DFS) Direct Traversal

#### Intuition
Use DFS to locate the target, then search for nodes at distance K. This involves two phases; first, determine the distance from the root to the target, then explore nodes at distance K using DFS.

#### Steps:
1. Perform a DFS to determine the depth of each node from the root and the path to the target node.
2. Once target is located, recursively search for nodes at the required distance.
3. Adjust distance computation as your search propagates back up recursively.

#### Code:
```typescript
function distanceK_v2(root: TreeNode | null, target: TreeNode | null, K: number): number[] {
  const result: number[] = [];

  function dfs(node: TreeNode | null, distance: number): number {
    if (!node) return -1;

    if (node === target) {
      subtreeAdd(node, 0); // Start adding nodes from target with distance 0
      return 1;
    }

    const left = dfs(node.left, K);
    const right = dfs(node.right, K);

    if (left !== -1) {
      if (left === K) result.push(node.val);
      subtreeAdd(node.right, left + 1);
      return left + 1;
    }

    if (right !== -1) {
      if (right === K) result.push(node.val);
      subtreeAdd(node.left, right + 1);
      return right + 1;
    }

    return -1;
  }

  function subtreeAdd(node: TreeNode | null, distance: number) {
    if (!node) return;

    if (distance === K) {
      result.push(node.val);
    } else {
      subtreeAdd(node.left, distance + 1);
      subtreeAdd(node.right, distance + 1);
    }
  }

  dfs(root, 0);
  return result;
}
```

#### Complexity:
- **Time Complexity**: O(N), since we are visiting each node at most twice.
- **Space Complexity**: O(N), accounting for the recursion stack in the worst case of a skewed tree.

