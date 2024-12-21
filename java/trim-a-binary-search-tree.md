## [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

### Approaches
- [Recursive DFS](#recursive-dfs)
- [Iterative DFS with Stack](#iterative-dfs-with-stack)

---

### Recursive DFS

**Intuition:**

The idea is to recursively traverse the tree and rebuild it by considering the constraints of a binary search tree (BST). For each node:

- If the node's value is less than `low`, then discard the left subtree (because all values in the left subtree are also less than `low`) and trim the right subtree.
- If the node's value is greater than `high`, discard the right subtree and trim the left subtree.
- If the node's value is within the range `[low, high]`, then keep the node and recursively trim both its left and right subtrees.

The base case for the recursion is when a `null` node is encountered.

**Time Complexity**: O(N) where N is the number of nodes in the tree. Each node is visited once.

**Space Complexity**: O(N) for the recursion stack in the worst-case scenario, which is a completely unbalanced tree.

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
    public TreeNode trimBST(TreeNode root, int low, int high) {
        // Base case: if the current node is null, return null
        if (root == null) {
            return null;
        }

        // If the current node's value is less than low,
        // we need to trim the left subtree and only consider the right subtree
        if (root.val < low) {
            return trimBST(root.right, low, high);
        }

        // If the current node's value is greater than high,
        // we need to trim the right subtree and only consider the left subtree
        if (root.val > high) {
            return trimBST(root.left, low, high);
        }

        // If the current node's value is within the range,
        // recursively trim both the left and right subtrees
        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);

        // Return the root, as it's within the range
        return root;
    }
}
```

---

### Iterative DFS with Stack

**Intuition:**

This approach uses an iterative Depth-First Search (DFS) strategy with a stack to avoid the recursion overhead. The idea is the same as the recursive approach, but maintains a stack to process the nodes iteratively.

- Initialize a stack and push the root node.
- Pop elements from the stack, check if the current node is within the range, and adjust its children nodes accordingly.
- Continue while the stack is not empty.

**Time Complexity**: O(N) since we process each node once.

**Space Complexity**: O(N) due to the stack which in the worst case can grow as large as the number of nodes in the tree.

```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        // Edge case: if the tree is empty
        if (root == null) {
            return null;
        }

        // Adjust the root to be within range
        while (root != null && (root.val < low || root.val > high)) {
            if (root.val < low) {
                root = root.right;
            } else if (root.val > high) {
                root = root.left;
            }
        }

        // Use a stack to perform DFS iteratively
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();

            if (node != null) {
                // Trim the left subtree if necessary
                while (node.left != null && node.left.val < low) {
                    node.left = node.left.right;
                }

                // Trim the right subtree if necessary
                while (node.right != null && node.right.val > high) {
                    node.right = node.right.left;
                }

                // Push children onto the stack to continue trimming other nodes
                stack.push(node.left);
                stack.push(node.right);
            }
        }

        return root;
    }
}
```

These solutions effectively demonstrate how to handle binary search tree modifications based on value constraints, using both recursive and iterative methods.

