# [Leetcode 114: Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

## Approaches
- [Approach 1: Recursion (Preorder Traversal)](#approach-1-recursion-preorder-traversal)
- [Approach 2: Iterative using Stack](#approach-2-iterative-using-stack)
- [Approach 3: Morris Traversal (Optimal)](#approach-3-morris-traversal-optimal)

### Approach 1: Recursion (Preorder Traversal)

**Intuition**

The idea is to perform a preorder traversal of the tree and rearrange the nodes as we visit them. In a preorder traversal, we visit the root first, then recursively do a preorder traversal of the left subtree, followed by a recursive preorder traversal of the right subtree.

We can accomplish this by making use of a helper function that flattens the left and right subtrees and then connects them to the root in the desired order.

**Algorithm**

1. Define a recursive helper function, passing the root of the tree as a parameter.
2. If the root is null, return immediately.
3. Recursively flatten the left subtree.
4. Recursively flatten the right subtree.
5. Save the right child of the root.
6. Move the flattened left subtree to the right of the root.
7. Set the left child of the root to null.
8. Attach the flattened right subtree to the end of the currently flattened tree.

```csharp
public class Solution {
    public void Flatten(TreeNode root) {
        // Flatten the tree starting from the root
        FlattenHelper(root);
    }
    
    private TreeNode FlattenHelper(TreeNode root) {
        if (root == null) return null;

        // Flatten the left and right subtrees
        TreeNode leftTail = FlattenHelper(root.left);
        TreeNode rightTail = FlattenHelper(root.right);

        // If there was a left subtree, we attach it between the root and the right subtree
        if (leftTail != null) {
            leftTail.right = root.right;
            root.right = root.left;
            root.left = null;
        }

        // we need to return the "right-most" node after we flattened this root
        if (rightTail != null) return rightTail;
        if (leftTail != null) return leftTail;

        // If it's a leaf node, return itself
        return root;
    }
}
```

**Time Complexity:** O(n), where n is the number of nodes in the tree, since we visit each node once.

**Space Complexity:** O(n) in the worst case because of the recursion stack.


### Approach 2: Iterative using Stack

**Intuition**

Instead of using recursion, we can utilize an iterative approach with a stack to keep track of nodes. A stack helps us mimic the recursion call stack but in an iterative way.

**Algorithm**

1. Initialize a stack and push the root node onto the stack.
2. While the stack is not empty, do the following:
   - Pop the top node from the stack.
   - If the node has a right child, push the right child onto the stack.
   - If the node has a left child, push the left child onto the stack.
   - Set the right child of the node to the topmost element of the stack, and set the left child to null.

```csharp
public class Solution {
    public void Flatten(TreeNode root) {
        if (root == null) return;
        
        // Create a stack and push the root node onto it
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.Push(root);
        
        while (stack.Count > 0) {
            TreeNode current = stack.Pop();

            // If the current node has a right child, push it onto the stack
            if (current.right != null) {
                stack.Push(current.right);
            }

            // If the current node has a left child, push it onto the stack
            if (current.left != null) {
                stack.Push(current.left);
            }

            // Attach the nodes in preorder traversal order
            if (stack.Count > 0) {
                current.right = stack.Peek();
            }
            current.left = null; // Don't forget to set the left to NULL
        }
    }
}
```

**Time Complexity:** O(n), where n is the number of nodes in the tree.

**Space Complexity:** O(n) as we are using a stack to store nodes.

### Approach 3: Morris Traversal (Optimal)

**Intuition**

Morris Traversal optimizes the approach by circumventing the use of a stack or recursion, thus achieving constant space complexity. The idea is to utilize the tree's structure to navigate the tree while flattening it in-place.

**Algorithm**

1. Initialize the current node as root.
2. While the current node is not null:
   - If the current node has a left child:
     - Find the rightmost node in the left subtree (inorder predecessor).
     - Link the rightmost node's right to the current node's right child.
     - Move the current node's left subtree to its right.
     - Set the current node's left to null.
   - Move to the right child of the current node.

```csharp
public class Solution {
    public void Flatten(TreeNode root) {
        TreeNode current = root;

        while (current != null) {
            if (current.left != null) {
                TreeNode pre = current.left;

                // Find the right-most node in the left subtree
                while (pre.right != null) {
                    pre = pre.right;
                }

                // Connect the right child of the right-most node
                pre.right = current.right;

                // Move the left subtree to the right
                current.right = current.left;
                current.left = null;
            }

            // Move to the right in the flattened tree
            current = current.right;
        }
    }
}
```

**Time Complexity:** O(n), where n is the number of nodes.

**Space Complexity:** O(1), as we've eliminated the need for a stack or recursion.

