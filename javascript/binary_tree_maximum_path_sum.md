# 124. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approach: Depth-First Search (DFS)

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

class Solution {
    constructor() {
        this.maxSum = Number.MIN_SAFE_INTEGER;
    }

    maxPathSum(root) {
        this.calculateMaxPath(root);
        return this.maxSum;
    }

    calculateMaxPath(node) {
        if (node === null) {
            return 0; // Base case: null node contributes 0 to the path sum
        }

        // Recursively calculate the maximum path sum of the left and right subtrees
        const leftMax = Math.max(0, this.calculateMaxPath(node.left));  // Ignore negative sums
        const rightMax = Math.max(0, this.calculateMaxPath(node.right)); // Ignore negative sums

        // Update the global maximum path sum
        this.maxSum = Math.max(this.maxSum, leftMax + rightMax + node.val);

        // Return the maximum path sum including the current node and one of its subtrees
        return Math.max(leftMax, rightMax) + node.val;
    }
}
```


