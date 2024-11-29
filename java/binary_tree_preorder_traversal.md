# 144. [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approach 1: Recursive Preorder Traversal

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorder(root, result);
        return result;
    }

    private void preorder(TreeNode node, List<Integer> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        result.add(node.val); // Visit the root
        preorder(node.left, result); // Recurse for the left subtree
        preorder(node.right, result); // Recurse for the right subtree
    }
}
```

## Approach 2: Iterative Preorder Traversal Using Stack

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result; // Return empty result if the tree is empty
        }

        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode currentNode = stack.pop();
            result.add(currentNode.val); // Visit the root

            // Push right child first so that left child is processed first
            if (currentNode.right != null) {
                stack.push(currentNode.right);
            }
            if (currentNode.left != null) {
                stack.push(currentNode.left);
            }
        }

        return result;
    }
}
```

## Approach 3: Iterative Preorder Traversal Using Stack

### Solution
```java
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        TreeNode current = root;

        while (current != null) {
            if (current.left == null) {
                result.add(current.val); // Visit the root
                current = current.right; // Move to the right
            } else {
                TreeNode predecessor = current.left;

                // Find the rightmost node in the left subtree
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }

                if (predecessor.right == null) {
                    result.add(current.val); // Visit the root
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