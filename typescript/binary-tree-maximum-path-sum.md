# [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

In this problem, we are given a non-empty binary tree, and we need to find a path in the tree where the sum of the node values is maximized. The path must go from any node to any other node in the tree.

This is a classic problem that can be tackled using various approaches depending on one's familiarity with trees and dynamic programming concepts.

### Approaches:
1. [Recursive DFS Approach](#recursive-dfs-approach)
2. [Optimized Recursive DFS Approach with Accumulated Maximum](#optimized-recursive-dfs-approach-with-accumulated-maximum)

## Recursive DFS Approach

### Intuition:
The goal is to explore all possible paths in the tree, where a path is defined as a sequence of nodes connected by edges. We can use Depth-First Search (DFS) to explore each possible path starting from every node. As we explore, we keep track of the maximum sum encountered.

### Steps:
1. Use a recursive helper function to calculate the maximum path sum starting from each node.
2. For each node, consider three cases:
   - Node's value itself.
   - Node's value + maximum sum from its left subtree.
   - Node's value + maximum sum from its right subtree.
3. Update the maximum path sum by taking the maximum of the current path sum and the maximum path sum from the children.
4. Return the maximum path sum attained including the current node as part of the path.

### Code:

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

function maxPathSum(root: TreeNode | null): number {
    let maxSum = Number.MIN_SAFE_INTEGER;

    function dfs(node: TreeNode | null): number {
        if (node === null) return 0;

        // Explore left and right subtrees
        const leftMax = Math.max(dfs(node.left), 0); // Ignore negative paths
        const rightMax = Math.max(dfs(node.right), 0); // Ignore negative paths

        // Current path sum with node included
        const currentPathSum = node.val + leftMax + rightMax;

        // Update maxSum, considering the current path
        maxSum = Math.max(maxSum, currentPathSum);

        // Return the maximum path sum including this node
        return node.val + Math.max(leftMax, rightMax);
    }

    dfs(root);

    return maxSum;
}
```

### Time Complexity:
- **O(n)**: Every node is visited once.

### Space Complexity:
- **O(h)**: Call stack space in the worst case (h is the height of the tree).

## Optimized Recursive DFS Approach with Accumulated Maximum

### Intuition:
The Recursive DFS Approach is already quite optimal for this problem since it visits each node once. However, we ensure that at each step, we update the global maximum path sum with the potential maximum sum from the current node's perspective. Thus, each recursion not only calculates the path sums of subtrees but also considers if the path sum through the current node results in a higher maximum value.

### Code:

This solution is already optimized as discussed above; hence the same code applies. The main idea is to handle the negative path sums and consider the node and its respective left and right choices effectively to keep track of the global maximum.

### Time Complexity:
- **O(n)**: Every node is visited once.

### Space Complexity:
- **O(h)**: Call stack space in the worst case (h is the height of the tree). 

In conclusion, using a recursive DFS approach allows us to efficiently find the maximum path sum while ensuring we only consider optimal paths for each subtree. This problem is an excellent example of using recursion and dynamic programming concepts to tackle complex tree problems.

