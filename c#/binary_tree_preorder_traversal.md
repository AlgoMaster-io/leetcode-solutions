# 144. [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approach 1: Recursive Preorder Traversal

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
using System.Collections.Generic;

public class Solution {
    public IList<int> PreorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        Preorder(root, result);
        return result;
    }

    private void Preorder(TreeNode node, List<int> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        result.Add(node.val); // Visit the root
        Preorder(node.left, result); // Recurse for the left subtree
        Preorder(node.right, result); // Recurse for the right subtree
    }
}
```

## Approach 2: Iterative Preorder Traversal Using Stack

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
using System.Collections.Generic;

public class Solution {
    public IList<int> PreorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        if (root == null) {
            return result; // Return empty result if the tree is empty
        }

        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.Push(root);

        while (stack.Count > 0) {
            TreeNode currentNode = stack.Pop();
            result.Add(currentNode.val); // Visit the root

            // Push right child first so that left child is processed first
            if (currentNode.right != null) {
                stack.Push(currentNode.right);
            }
            if (currentNode.left != null) {
                stack.Push(currentNode.left);
            }
        }

        return result;
    }
}
```

## Approach 3: Morris Preorder Traversal

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
using System.Collections.Generic;

public class Solution {
    public IList<int> PreorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        TreeNode current = root;

        while (current != null) {
            if (current.left == null) {
                result.Add(current.val); // Visit the root
                current = current.right; // Move to the right
            } else {
                TreeNode predecessor = current.left;

                // Find the rightmost node in the left subtree
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }

                if (predecessor.right == null) {
                    result.Add(current.val); // Visit the root
                    predecessor.right = current; // Create a temporary thread to the root
                    current = current.left; // Move to the left
                } else {
                    predecessor.right = null; // Remove the temporary thread
                    current = current.right; // Move to the right
                }
            }
        }

        return result;
    }
}
```

