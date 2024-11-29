# 98. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Approach 1: Inorder Traversal (Iterative)

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
using System.Collections.Generic;

public class Solution {
    public bool IsValidBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode current = root;
        TreeNode prev = null;

        while (current != null || stack.Count > 0) {
            while (current != null) {
                stack.Push(current); // Push nodes of the left subtree
                current = current.left;
            }

            current = stack.Pop(); // Process the top node

            if (prev != null && current.val <= prev.val) {
                return false; // If the current value is not greater than the previous, it's invalid
            }
            prev = current;

            current = current.right; // Move to the right subtree
        }

        return true;
    }
}
```

## Approach 2: Recursive Inorder Traversal

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
public class Solution {
    private TreeNode prev = null;

    public bool IsValidBST(TreeNode root) {
        return Inorder(root);
    }

    private bool Inorder(TreeNode node) {
        if (node == null) {
            return true; // Base case: empty tree is valid
        }

        if (!Inorder(node.left)) {
            return false; // Left subtree is invalid
        }

        if (prev != null && node.val <= prev.val) {
            return false; // Current value is not greater than the previous
        }
        prev = node; // Update the previous node

        return Inorder(node.right); // Recurse for the right subtree
    }
}
```

## Approach 3: DFS with Range Validation

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
public class Solution {
    public bool IsValidBST(TreeNode root) {
        return Validate(root, null, null);
    }

    private bool Validate(TreeNode node, int? lower, int? upper) {
        if (node == null) {
            return true; // Base case: empty tree is valid
        }

        if ((lower.HasValue && node.val <= lower.Value) || (upper.HasValue && node.val >= upper.Value)) {
            return false; // Node value violates the range constraints
        }

        // Recursively validate the left and right subtrees
        return Validate(node.left, lower, node.val) && Validate(node.right, node.val, upper);
    }
}
```

