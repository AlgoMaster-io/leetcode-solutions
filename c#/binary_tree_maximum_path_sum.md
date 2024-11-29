# 124. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approach: Depth-First Search (DFS)

### Solution
c#
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
public class Solution {
    private int maxSum = int.MinValue;

    public int MaxPathSum(TreeNode root) {
        CalculateMaxPath(root);
        return maxSum;
    }

    private int CalculateMaxPath(TreeNode node) {
        if (node == null) {
            return 0; // Base case: null node contributes 0 to the path sum
        }

        // Recursively calculate the maximum path sum of the left and right subtrees
        int leftMax = Math.Max(0, CalculateMaxPath(node.left));  // Ignore negative sums
        int rightMax = Math.Max(0, CalculateMaxPath(node.right)); // Ignore negative sums

        // Update the global maximum path sum
        maxSum = Math.Max(maxSum, leftMax + rightMax + node.val);

        // Return the maximum path sum including the current node and one of its subtrees
        return Math.Max(leftMax, rightMax) + node.val;
    }
}

