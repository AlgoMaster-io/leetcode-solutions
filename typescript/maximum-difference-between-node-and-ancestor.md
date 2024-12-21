## [Leetcode 1026: Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/)

### Navigation

- [Approach 1: Brute Force using Depth-First Search (DFS)](#approach-1)
- [Approach 2: Optimized Depth-First Search](#approach-2)

---

### Approach 1: Brute Force using Depth-First Search (DFS)

**Intuition**

The brute force method involves exploring all possible paths from each node to all its descendants and calculating the maximum difference between the node and its descendants. We essentially do a pre-order traversal where at each node, we evaluate the difference for each of its descendant nodes.

**TypeScript Code**

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val)
        this.left = (left===undefined ? null : left)
        this.right = (right===undefined ? null : right)
    }
}

function maxAncestorDiff(root: TreeNode | null): number {
    // Helper function for DFS traversal
    function dfs(node: TreeNode | null, ancestors: number[]): number {
        if (!node) return 0;

        // Calculate the differences with all ancestors
        let maxDiff = 0;
        for (let ancestor of ancestors) {
            maxDiff = Math.max(maxDiff, Math.abs(node.val - ancestor));
        }

        // Add current node value to the list of ancestors
        ancestors.push(node.val);

        // Recursively find differences in left and right subtrees
        let leftDiff = dfs(node.left, ancestors);
        let rightDiff = dfs(node.right, ancestors);

        // Backtracking step: remove current node from ancestor list
        ancestors.pop();

        // Return the maximum difference found
        return Math.max(maxDiff, leftDiff, rightDiff);
    }

    return dfs(root, []);
}
```

**Complexity Analysis**

- **Time Complexity**: O(N^2), where N is the number of nodes in the tree. For each node, we potentially traverse its ancestor list entirely.
- **Space Complexity**: O(N), due to the stack space used for recursion and storing ancestor values.

---

### Approach 2: Optimized Depth-First Search

**Intuition**

We can optimize the previous approach by maintaining only the minimum and maximum ancestor values observed so far on the current path. At each node, the difference between the node's value and these two extremes gives the maximum possible difference at that node. This reduces redundancy by avoiding list operations and allows direct computation of the needed results.

**TypeScript Code**

```typescript
function maxAncestorDiffOptimized(root: TreeNode | null): number {
    function dfs(node: TreeNode | null, minAncestor: number, maxAncestor: number): number {
        if (!node) return 0;

        // Calculate the potential differences at this node
        const currentMaxDiff = Math.max(
            Math.abs(node.val - minAncestor),
            Math.abs(node.val - maxAncestor)
        );

        // Update min and max ancestors
        const newMin = Math.min(minAncestor, node.val);
        const newMax = Math.max(maxAncestor, node.val);

        // Continue DFS for left and right subtrees
        const leftMaxDiff = dfs(node.left, newMin, newMax);
        const rightMaxDiff = dfs(node.right, newMin, newMax);

        // Return the maximum difference found
        return Math.max(currentMaxDiff, leftMaxDiff, rightMaxDiff);
    }

    return dfs(root, root.val, root.val);
}
```

**Complexity Analysis**

- **Time Complexity**: O(N), where N is the number of nodes in the tree, since we're visiting each node exactly once.
- **Space Complexity**: O(H), where H is the height of the tree, due to the stack space used for recursion. In the worst case (skewed tree), H could be O(N).

