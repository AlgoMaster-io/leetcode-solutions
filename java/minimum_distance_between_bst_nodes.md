# 783. [Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Approach 1: Inorder Traversal with a List

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the inorder traversal
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int minDiffInBST(TreeNode root) {
        List<Integer> values = new ArrayList<>();
        inorder(root, values);

        int minDiff = Integer.MAX_VALUE;
        for (int i = 1; i < values.size(); i++) {
            minDiff = Math.min(minDiff, values.get(i) - values.get(i - 1));
        }

        return minDiff;
    }

    private void inorder(TreeNode node, List<Integer> values) {
        if (node == null) {
            return;
        }

        inorder(node.left, values); // Recurse for the left subtree
        values.add(node.val); // Add the current node value
        inorder(node.right, values); // Recurse for the right subtree
    }
}
```

## Approach 2: Inorder Traversal with Constant Space

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
public class Solution {
    private Integer prev = null;
    private int minDiff = Integer.MAX_VALUE;

    public int minDiffInBST(TreeNode root) {
        inorder(root);
        return minDiff;
    }

    private void inorder(TreeNode node) {
        if (node == null) {
            return;
        }

        inorder(node.left); // Recurse for the left subtree

        if (prev != null) {
            minDiff = Math.min(minDiff, node.val - prev); // Calculate the difference with the previous value
        }
        prev = node.val; // Update the previous node value

        inorder(node.right); // Recurse for the right subtree
    }
}
```

## Approach 3: Morris Traversal (Space Optimized)

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
public class Solution {
    public int minDiffInBST(TreeNode root) {
        TreeNode current = root;
        TreeNode prev = null;
        int minDiff = Integer.MAX_VALUE;

        while (current != null) {
            if (current.left == null) {
                // Process the current node
                if (prev != null) {
                    minDiff = Math.min(minDiff, current.val - prev.val);
                }
                prev = current;
                current = current.right; // Move to the right subtree
            } else {
                TreeNode predecessor = current.left;

                // Find the rightmost node in the left subtree
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }

                if (predecessor.right == null) {
                    predecessor.right = current; // Create a temporary thread to the root
                    current = current.left; // Move to the left subtree
                } else {
                    predecessor.right = null; // Remove the temporary thread
                    if (prev != null) {
                        minDiff = Math.min(minDiff, current.val - prev.val);
                    }
                    prev = current;
                    current = current.right; // Move to the right subtree
                }
            }
        }

        return minDiff;
    }
}
```