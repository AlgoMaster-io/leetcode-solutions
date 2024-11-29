# 124. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approach: Depth-First Search (DFS)

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.left = (left === undefined ? null : left);
        this.right = (right === undefined ? null : right);
    }
}

function maxPathSum(root: TreeNode | null): number {
    let maxSum = -Infinity;

    function calculateMaxPath(node: TreeNode | null): number {
        if (node === null) {
            return 0; // Base case: null node contributes 0 to the path sum
        }

        // Recursively calculate the maximum path sum of the left and right subtrees
        const leftMax = Math.max(0, calculateMaxPath(node.left));  // Ignore negative sums
        const rightMax = Math.max(0, calculateMaxPath(node.right)); // Ignore negative sums

        // Update the global maximum path sum
        maxSum = Math.max(maxSum, leftMax + rightMax + node.val);

        // Return the maximum path sum including the current node and one of its subtrees
        return Math.max(leftMax, rightMax) + node.val;
    }

    calculateMaxPath(root);
    return maxSum;
}
```

