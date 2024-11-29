# 94. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approach 1: Recursive Inorder Traversal

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
using System.Collections.Generic;

public class Solution {
    public IList<int> InorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        Inorder(root, result);
        return result;
    }

    private void Inorder(TreeNode node, List<int> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        Inorder(node.left, result); // Recurse for the left subtree
        result.Add(node.val); // Visit the root
        Inorder(node.right, result); // Recurse for the right subtree
    }
}
```

## Approach 2: Iterative Inorder Traversal Using Stack

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
using System.Collections.Generic;

public class Solution {
    public IList<int> InorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode current = root;

        while (current != null || stack.Count > 0) {
            while (current != null) {
                stack.Push(current); // Push the current node and move to the left subtree
                current = current.left;
            }

            current = stack.Pop(); // Process the top node
            result.Add(current.val); // Visit the node
            current = current.right; // Move to the right subtree
        }

        return result;
    }
}
```

## Approach 3: Morris Inorder Traversal (Without Recursion or Stack)

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
using System.Collections.Generic;

public class Solution {
    public IList<int> InorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        TreeNode current = root;

        while (current != null) {
            if (current.left == null) {
                result.Add(current.val); // Visit the node
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
                    result.Add(current.val); // Visit the node
                    current = current.right; // Move to the right subtree
                }
            }
        }

        return result;
    }
}
```

