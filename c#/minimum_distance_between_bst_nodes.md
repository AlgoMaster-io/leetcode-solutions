# 783. [Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Approach 1: Inorder Traversal with a List

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the inorder traversal
using System;
using System.Collections.Generic;

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int val = 0, TreeNode left = null, TreeNode right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

public class Solution {
    public int MinDiffInBST(TreeNode root) {
        List<int> values = new List<int>();
        Inorder(root, values);

        int minDiff = int.MaxValue;
        for (int i = 1; i < values.Count; i++) {
            minDiff = Math.Min(minDiff, values[i] - values[i - 1]);
        }

        return minDiff;
    }

    private void Inorder(TreeNode node, List<int> values) {
        if (node == null) {
            return;
        }

        Inorder(node.left, values); // Recurse for the left subtree
        values.Add(node.val); // Add the current node value
        Inorder(node.right, values); // Recurse for the right subtree
    }
}
```

## Approach 2: Inorder Traversal with Constant Space

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
public class Solution {
    private int? prev = null;
    private int minDiff = int.MaxValue;

    public int MinDiffInBST(TreeNode root) {
        Inorder(root);
        return minDiff;
    }

    private void Inorder(TreeNode node) {
        if (node == null) {
            return;
        }

        Inorder(node.left); // Recurse for the left subtree

        if (prev.HasValue) {
            minDiff = Math.Min(minDiff, node.val - prev.Value); // Calculate the difference with the previous value
        }
        prev = node.val; // Update the previous node value

        Inorder(node.right); // Recurse for the right subtree
    }
}
```

## Approach 3: Morris Traversal (Space Optimized)

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
public class Solution {
    public int MinDiffInBST(TreeNode root) {
        TreeNode current = root;
        TreeNode prev = null;
        int minDiff = int.MaxValue;

        while (current != null) {
            if (current.left == null) {
                // Process the current node
                if (prev != null) {
                    minDiff = Math.Min(minDiff, current.val - prev.val);
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
                        minDiff = Math.Min(minDiff, current.val - prev.val);
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

