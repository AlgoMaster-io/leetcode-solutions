# 145. [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approach 1: Recursive Postorder Traversal

### Solution
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
using System.Collections.Generic;

public class Solution {
    public IList<int> PostorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        Postorder(root, result);
        return result;
    }

    private void Postorder(TreeNode node, List<int> result) {
        if (node == null) {
            return; // Base case: if the node is null, return
        }

        Postorder(node.left, result); // Recurse for the left subtree
        Postorder(node.right, result); // Recurse for the right subtree
        result.Add(node.val); // Visit the root
    }
}
```

## Approach 2: Iterative Postorder Traversal Using Two Stacks

### Solution
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(2n) = O(n), for two stacks
using System.Collections.Generic;

public class Solution {
    public IList<int> PostorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        if (root == null) {
            return result;
        }

        Stack<TreeNode> stack1 = new Stack<TreeNode>();
        Stack<TreeNode> stack2 = new Stack<TreeNode>();
        stack1.Push(root);

        while (stack1.Count != 0) {
            TreeNode current = stack1.Pop();
            stack2.Push(current);

            if (current.left != null) {
                stack1.Push(current.left); // Push left child to stack1
            }
            if (current.right != null) {
                stack1.Push(current.right); // Push right child to stack1
            }
        }

        while (stack2.Count != 0) {
            result.Add(stack2.Pop().val); // Add nodes in postorder from stack2
        }

        return result;
    }
}
```

## Approach 3: Iterative Postorder Traversal Using One Stack

### Solution
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
using System.Collections.Generic;

public class Solution {
    public IList<int> PostorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        if (root == null) {
            return result;
        }

        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode current = root;
        TreeNode lastVisited = null;

        while (current != null || stack.Count != 0) {
            if (current != null) {
                stack.Push(current); // Push nodes to the stack
                current = current.left; // Move to the left subtree
            } else {
                TreeNode peekNode = stack.Peek();
                if (peekNode.right != null && lastVisited != peekNode.right) {
                    current = peekNode.right; // Move to the right subtree
                } else {
                    result.Add(peekNode.val); // Visit the node
                    lastVisited = stack.Pop();
                }
            }
        }

        return result;
    }
}
```

## Approach 4: Morris Postorder Traversal (Space Optimized)

### Solution
```csharp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
using System.Collections.Generic;

public class Solution {
    public IList<int> PostorderTraversal(TreeNode root) {
        List<int> result = new List<int>();
        TreeNode dummy = new TreeNode(0);
        dummy.left = root;
        TreeNode current = dummy, predecessor = null;

        while (current != null) {
            if (current.left == null) {
                current = current.right;
            } else {
                predecessor = current.left;
                while (predecessor.right != null && predecessor.right != current) {
                    predecessor = predecessor.right;
                }

                if (predecessor.right == null) {
                    predecessor.right = current; // Create a temporary thread
                    current = current.left;
                } else {
                    ReverseAddPath(current.left, predecessor, result);
                    predecessor.right = null; // Remove the temporary thread
                    current = current.right;
                }
            }
        }

        return result;
    }

    private void ReverseAddPath(TreeNode start, TreeNode end, List<int> result) {
        List<int> temp = new List<int>();
        while (start != end) {
            temp.Add(start.val);
            start = start.right;
        }
        temp.Add(end.val);
        for (int i = temp.Count - 1; i >= 0; i--) {
            result.Add(temp[i]); // Add nodes in reverse order
        }
    }
}
```


