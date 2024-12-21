# [Leetcode 199: Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

## Approaches:
- [Approach 1: Depth-First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Optimized DFS with Level Tracking](#approach-3-optimized-dfs-with-level-tracking)

### Approach 1: Depth-First Search (DFS)

**Intuition:**
In this approach, we make use of the DFS strategy to traverse the binary tree. By exploring the rightmost node first at every level, we can capture the nodes visible from the right side.

**Steps:**
1. We'll utilize a recursive function to perform DFS, starting from the root.
2. For each node, first visit the right child, and then visit the left child.
3. Keep track of the maximum depth encountered so far. If the current depth exceeds the recorded maximal depth, this means we have encountered a new "visible" node from the right.

**Time Complexity:** O(N) - where N is the number of nodes.
**Space Complexity:** O(H) - where H is the height of the tree, due to the stack space in recursion.

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

function rightSideView(root: TreeNode | null): number[] {
    const result: number[] = [];

    function dfs(node: TreeNode | null, depth: number): void {
        if (!node) return;

        // If this is the first time we've reached this depth,
        // we're seeing the rightmost node at this level
        if (depth === result.length) {
            result.push(node.val);
        }

        // Recursive calls
        dfs(node.right, depth + 1);  // Explore right first
        dfs(node.left, depth + 1);   // Then left
    }

    dfs(root, 0);
    return result;
}
```

### Approach 2: Breadth-First Search (BFS)

**Intuition:**
A BFS approach involves using a queue to traverse each level of the tree from left to right. Since we are interested in the right side view, while processing each level, we only need the last node value.

**Steps:**
1. Initialize a queue with the root node and perform a level order traversal.
2. For each level, save the value of the last node from the queue to the result list.

**Time Complexity:** O(N) - as every node is processed once.
**Space Complexity:** O(maxWidth) - maximum number of nodes at any level.

```typescript
function rightSideViewBFS(root: TreeNode | null): number[] {
    if (!root) return [];

    const result: number[] = [];
    const queue: TreeNode[] = [root];

    while (queue.length > 0) {
        let levelSize = queue.length;
        let currentNode: TreeNode | null = null;

        for (let i = 0; i < levelSize; i++) {
            currentNode = queue.shift()!;

            if (currentNode.left) {
                queue.push(currentNode.left);
            }
            if (currentNode.right) {
                queue.push(currentNode.right);
            }
        }
        // Last node processed in this level
        result.push(currentNode.val);
    }

    return result;
}
```

### Approach 3: Optimized DFS with Level Tracking

**Intuition:**
This optimized DFS approach involves tracking the current level and updating the result directly without overlapping checks present in naive DFS.

**Steps:**
1. Similar to the standard DFS approach, start from the root.
2. Maintain the current depth and update the result if you encounter the first node in this depth.
3. We still explore the right nodes first to ensure they appear as the first visit for the level.

**Time Complexity:** O(N)
**Space Complexity:** O(H)

```typescript
function rightSideViewOptimizedDFS(root: TreeNode | null): number[] {
    const result: number[] = [];
    
    function dfs(node: TreeNode | null, depth: number): void {
        if (!node) return;

        // Capture the first node at a new depth as seen from the right
        if (depth === result.length) {
            result[depth] = node.val;
        }
        
        dfs(node.right, depth + 1);
        dfs(node.left, depth + 1);
    }

    dfs(root, 0);
    return result;
}
```

Each approach offers a different perspective on how to access the potentially visible nodes from the right side of a binary tree, balancing between the detail and efficiency of node traversal.

