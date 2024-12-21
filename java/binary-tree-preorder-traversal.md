# [Leetcode 144: Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approaches

1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Stack](#iterative-approach-using-stack)
3. [Morris Traversal (Most Optimal)](#morris-traversal-most-optimal)

## Recursive Approach

### Intuition
The recursive approach is the most straightforward way to perform a preorder traversal of a binary tree. In a preorder traversal, the root node is visited first, followed by the left subtree, and finally the right subtree. The recursive implementation mimics this order naturally by recursively calling the preorder function for each subtree.

### Approach
- Visit the root node first.
- Recursively traverse the left subtree.
- Recursively traverse the right subtree.

### Code
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
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorder(root, result);
        return result;
    }
    
    private void preorder(TreeNode node, List<Integer> result) {
        if (node == null) {
            return;
        }
        // Add the root node to the result list
        result.add(node.val);
        // Recur on the left child
        preorder(node.left, result);
        // Recur on the right child
        preorder(node.right, result);
    }
}
```

### Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the binary tree, as we visit each node once.
- **Space Complexity**: O(n) in the worst case due to the recursion stack, and O(log n) for a balanced tree.

## Iterative Approach using Stack

### Intuition
An iterative approach using a stack is a common way to avoid recursion and control the function call behavior explicitly. The idea is to mimic the system's call stack with an explicit stack data structure to store nodes yet to be processed.

### Approach
- Use a stack to keep track of nodes. Initially, push the root node to the stack.
- While the stack is not empty, pop a node from the stack, add its value to the result list, and then push its right and left children to the stack (in that order).
- This ensures that nodes are processed in preorder sequence.

### Code
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode current = stack.pop();
            result.add(current.val);
            
            // Push right child first so that left child is processed first
            if (current.right != null) {
                stack.push(current.right);
            }
            if (current.left != null) {
                stack.push(current.left);
            }
        }
        
        return result;
    }
}
```

### Complexity
- **Time Complexity**: O(n), since each node is processed (visited and added to the result list) once.
- **Space Complexity**: O(n), in the worst case, due to the stack holding all nodes (in the case of a skewed tree). In the best case of a balanced tree, it's O(log n).

## Morris Traversal (Most Optimal)

### Intuition
Morris Traversal modifies the tree structure temporarily to allow traversal of the tree without using additional space or a stack. It uses the concept of threading a binary tree.

### Approach
- Start with the root node and initialize the `current` node to root.
- While the current node is not null:
  - If the current node has no left child, add its value to the result and move to its right child.
  - If the current node has a left child, find the rightmost node in the left subtree (inorder predecessor).
  - If the rightmost node's right child is null, set its right child to the current node, and move to the left child.
  - If the rightmost node's right child is the current node (thread already exists), set it back to null (restoring tree structure), add the current node's value to result, and move to its right child.

### Code
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        TreeNode current = root;
        
        while (current != null) {
            if (current.left == null) {
                // If no left child, add current node's value to result
                result.add(current.val);
                // Move to right child
                current = current.right;
            } else {
                // Find the inorder predecessor of current
                TreeNode predecessor = current.left;
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }
                
                if (predecessor.right == null) {
                    // Establish thread for redirecting back from left subtree
                    predecessor.right = current;
                    // Add current not to the result before going to left child
                    result.add(current.val);
                    current = current.left;
                } else {
                    // Remove the thread
                    predecessor.right = null;
                    // Move to right child
                    current = current.right;
                }
            }
        }
        
        return result;
    }
}
```

### Complexity
- **Time Complexity**: O(n), as each edge and node is visited at most twice (the second time goes without child links).
- **Space Complexity**: O(1), as no additional data structures or recursion stacks are used beyond the result list.

