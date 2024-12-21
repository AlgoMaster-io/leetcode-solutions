# [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approaches
1. [Naive Recursive Approach](#naive-recursive-approach)
2. [Optimized Recursive Approach with Postorder Traversal](#optimized-recursive-approach)

### Naive Recursive Approach

The naive idea here is to consider each node of the binary tree, calculate the maximum path length traversing through it, and then take the largest of these lengths. The maximum path of a node is calculated as the sum of the heights of its left and right subtrees.

#### Detailed Intuition:

1. **Definition of Diameter**: For every node we consider, its diameter is defined as the number of nodes on the longest path between two leaves.
2. **Height Calculation**: The height of a node in this approach is determined by calculating the height of its left and right subtrees recursively.
3. **Brute Force Calculation**: For each node, calculate the diameter (left height + right height), and update the maximum diameter found so far.

#### Code:

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) return 0;
        // The diameter through root node
        int diameterThroughRoot = height(root.left) + height(root.right);
        // Recursively compute diameters of left and right subtrees
        int leftDiameter = diameterOfBinaryTree(root.left);
        int rightDiameter = diameterOfBinaryTree(root.right);
        // The final diameter is the maximum of these three
        return Math.max(diameterThroughRoot, Math.max(leftDiameter, rightDiameter));
    }

    // Compute the height of the tree
    private int height(TreeNode node) {
        if (node == null) return 0;
        return 1 + Math.max(height(node.left), height(node.right));
    }
}
```

#### Complexity:
- **Time Complexity**: O(N^2), since for every node, we calculate the height of the tree, a O(N) operation.
- **Space Complexity**: O(N), space required for recursion stack in the worst case (skewed tree).

### Optimized Recursive Approach

To optimize the naive approach, we can calculate the height of the tree while computing the diameter at the same time. This prevents re-calculation of the height, reducing redundant operations.

#### Detailed Intuition:

1. **Postorder Traversal**: Utilize postorder traversal, where we calculate the height of subtrees and the diameter while backtracking.
2. **Passing the Max Diameter**: Use a global or wrapper object to store the maximum diameter found during the traversal.

#### Code:

```java
class Solution {
    private int maxDiameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        calculateHeightAndDiameter(root);
        return maxDiameter;
    }

    private int calculateHeightAndDiameter(TreeNode node) {
        if (node == null) return 0;
        // Calculate the heights of left and right subtrees
        int leftHeight = calculateHeightAndDiameter(node.left);
        int rightHeight = calculateHeightAndDiameter(node.right);

        // Calculate the diameter passing through this node
        int diameterThroughNode = leftHeight + rightHeight;
        
        // Update the maximum diameter
        maxDiameter = Math.max(maxDiameter, diameterThroughNode);

        // Return height of the current node
        return 1 + Math.max(leftHeight, rightHeight);
    }
}
```

#### Complexity:
- **Time Complexity**: O(N), since we only pass through each node once.
- **Space Complexity**: O(N), due to recursion call stack (worst case for skewed tree).

By optimizing our approach, we reduced the time complexity from O(N^2) to O(N) by calculating the height and checking the diameter simultaneously.

