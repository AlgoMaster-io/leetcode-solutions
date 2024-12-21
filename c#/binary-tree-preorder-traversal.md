# [Leetcode 144: Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach Using Stack](#iterative-approach-using-stack)
3. [Morris Traversal (No Extra Space)](#morris-traversal-no-extra-space)

## Recursive Approach

The simplest way to solve the problem is by using recursion. The recursive approach follows the nature of preorder traversal directly: visit the root, traverse the left subtree, and then traverse the right subtree. 

### Intuition:

- Start by visiting the root node.
- Recursively perform preorder traversal on the left subtree.
- Recursively perform preorder traversal on the right subtree.

### Code:

```csharp
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
    public IList<int> PreorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        Preorder(root, result);
        return result;
    }
    
    // Helper function for recursion
    private void Preorder(TreeNode node, List<int> result) {
        if (node == null) return; // Base case: if node is null, return
        result.Add(node.val);     // Visit node
        Preorder(node.left, result);  // Traverse left subtree
        Preorder(node.right, result); // Traverse right subtree
    }
}
```

### Time Complexity:
- **O(n)**: Each node is visited once.
 
### Space Complexity:
- **O(n)**: Due to recursion stack in the worst case.

## Iterative Approach Using Stack

The recursive method can be replaced with an iterative approach using an explicit stack, which simulates the recursion's call stack.

### Intuition:

- Initialize a stack and push the root node.
- While the stack is not empty, process nodes by popping the stack:
  - Visit the node.
  - Push the right child first (so that the left child is processed first).
  - Push the left child.
  
### Code:

```csharp
public class Solution {
    public IList<int> PreorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        if (root == null) return result;

        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.Push(root);

        while (stack.Count > 0) {
            TreeNode node = stack.Pop();
            result.Add(node.val); // Visit node
            
            if (node.right != null) stack.Push(node.right); // Right child pushed first
            if (node.left != null) stack.Push(node.left);   // Left child pushed second
        }

        return result;
    }
}
```

### Time Complexity:
- **O(n)**: Each node is visited once.
 
### Space Complexity:
- **O(n)**: In the worst case, the stack will store all nodes.

## Morris Traversal (No Extra Space)

Morris Traversal offers a way of traversing the tree without utilizing extra space for a stack or recursion.

### Intuition:

- Maintain a current pointer initialized to root.
- While current is not null:
  - If current.left is null, visit current and move to current.right.
  - Otherwise, find the rightmost node in current's left subtree (predecessor).
    - If predecessor.right is null, set predecessor.right to current (thread it), and move to current.left.
    - If predecessor.right is current (already threaded), visit current, set predecessor.right back to null, and move to current.right.
    
### Code:

```csharp
public class Solution {
    public IList<int> PreorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        TreeNode current = root;

        while (current != null) {
            if (current.left == null) {
                result.Add(current.val); // Visit node
                current = current.right;
            } else {
                // Find the rightmost node in the left subtree
                TreeNode predecessor = current.left;
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }

                if (predecessor.right == null) {
                    // Thread the tree
                    result.Add(current.val); // Visit node
                    predecessor.right = current;
                    current = current.left;
                } else {
                    // Left subtree already visited
                    predecessor.right = null;
                    current = current.right;
                }
            }
        }

        return result;
    }
}
```

### Time Complexity:
- **O(n)**: Each node is visited a constant number of times.
 
### Space Complexity:
- **O(1)**: No extra space apart from variables. 

These approaches give you a range of options, from simple recursive to the more sophisticated Morris traversal, allowing efficient traversal without additional space.

