# [Leetcode 94: Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approaches
- [Recursive Inorder Traversal](#recursive-inorder-traversal)
- [Iterative Inorder Traversal Using Stack](#iterative-inorder-traversal-using-stack)
- [Morris Inorder Traversal (Optimal)](#morris-inorder-traversal-optimal)

### Recursive Inorder Traversal

The recursive approach to inorder traversal is conceptually simple and closely follows the mathematical definition of an inorder traversal, which visits the left subtree, the root, and then the right subtree.

**Intuition:**
1. If the current node is null, simply return (base case for recursion).
2. Recursively call the function for the left child node.
3. Visit the current node (process the node's value).
4. Recursively call the function for the right child node.

This approach leverages the system call stack to manage the traversal order, which inherently handles the backtracking for us.

**Time Complexity:** O(n) - As each node is visited once.

**Space Complexity:** O(n) in the worst case due to the recursion stack (when the tree is a linked list).

```csharp
// Definition for a binary tree node.
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public IList<int> InorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        InorderHelper(root, result);
        return result;
    }

    private void InorderHelper(TreeNode node, List<int> result) {
        if (node == null) return; // Base case
        InorderHelper(node.left, result); // Visit left subtree
        result.Add(node.val); // Visit root (current node)
        InorderHelper(node.right, result); // Visit right subtree
    }
}
```

### Iterative Inorder Traversal Using Stack

The iterative approach utilizes a stack to simulate the recursive traversal. This allows traversal without relying on the system's call stack.

**Intuition:**
1. Start at the root node and iterate until all nodes are processed.
2. Push each node onto a stack and keep traversing the left subtree until a null node is reached.
3. Pop a node from the stack, visit it, and then proceed to its right subtree.
4. Repeat until the stack is empty.

This method provides the same traversal order while manually managing the stack.

**Time Complexity:** O(n) - Every node is pushed and popped from the stack once.

**Space Complexity:** O(n) - The stack will hold all nodes in the worst case (skewed tree).

```csharp
public class Solution {
    public IList<int> InorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode current = root;

        while (current != null || stack.Count > 0) {
            // Reach the left most Node of the current Node
            while (current != null) {
                stack.Push(current); // Place pointer to a tree node on the stack
                current = current.left; // before traversing the node's left subtree
            }
            
            // Current must be null at this point
            current = stack.Pop();
            result.Add(current.val); // Add the node value to result

            // we have visited the node and its left subtree, now it's right subtree's turn
            current = current.right;
        }
        return result;
    }
}
```

### Morris Inorder Traversal (Optimal)

Morris Traversal is an advanced technique to achieve inorder traversal without using recursion or a stack, optimizing space utilization.

**Intuition:**
1. Initialize the current node as the root.
2. While the current node is not null:
   - If the current node does not have a left child, visit the node and move to its right child.
   - If the current node has a left child, find the inorder predecessor of the current node:
     - Connect the predecessor's right to the current node. Then move current to its left child.
     - If the predecessor's right is already connected to the current node, remove this connection, visit the current node, and move to its right child.

This method temporarily changes the tree structure to facilitate threading but restores it afterward.

**Time Complexity:** O(n) - As each edge is visited at most twice.

**Space Complexity:** O(1) - Uses no extra space other than variables.

```csharp
public class Solution {
    public IList<int> InorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        TreeNode current = root;
        
        while (current != null) {
            if (current.left == null) {
                // If there is no left child, visit this node and go right
                result.Add(current.val);
                current = current.right;
            } else {
                // Find the inorder predecessor of current
                TreeNode predecessor = current.left;
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }
                
                // Make current as the right child of its inorder predecessor if not done
                if (predecessor.right == null) {
                    predecessor.right = current;
                    current = current.left;
                }
                // If the threading is already present
                else {
                    predecessor.right = null; // Remove the thread
                    result.Add(current.val); // Visit the node
                    current = current.right; // Move to the right node
                }
            }
        }
        
        return result;
    }
}
```

This document provides solutions from basic recursive approach to optimal Morris traversal ensuring efficient space utilization and understanding of inorder traversal methodologies.

