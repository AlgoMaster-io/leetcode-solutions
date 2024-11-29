# 173. [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approach 1: Controlled Recursion Using Stack

### Solution
```csharp
// Time Complexity:
//   - Constructor: O(h), where h is the height of the tree
//   - Next(): O(1) on average across all calls
//   - HasNext(): O(1)
// Space Complexity: O(h), where h is the height of the tree for the stack
using System.Collections.Generic;

public class BSTIterator
{
    private Stack<TreeNode> stack;

    public BSTIterator(TreeNode root)
    {
        stack = new Stack<TreeNode>();
        PushLeft(root);
    }

    // Push all left children of the current node to the stack
    private void PushLeft(TreeNode node)
    {
        while (node != null)
        {
            stack.Push(node);
            node = node.left;
        }
    }

    /** @return the next smallest number */
    public int Next()
    {
        TreeNode node = stack.Pop(); // Get the topmost node (smallest available)
        if (node.right != null)
        {
            PushLeft(node.right); // Process the right subtree
        }
        return node.val;
    }

    /** @return whether we have a next smallest number */
    public bool HasNext()
    {
        return stack.Count > 0; // Check if the stack is non-empty
    }
}

public class TreeNode
{
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}
```

## Approach 2: Precomputed Inorder Traversal

### Solution
```csharp
// Time Complexity:
//   - Constructor: O(n), where n is the number of nodes in the tree
//   - Next(): O(1)
//   - HasNext(): O(1)
// Space Complexity: O(n), where n is the number of nodes in the tree
using System.Collections.Generic;

public class BSTIterator
{
    private List<int> inorder;
    private int index;

    public BSTIterator(TreeNode root)
    {
        inorder = new List<int>();
        index = 0;
        InorderTraversal(root);
    }

    // Perform an inorder traversal to store the elements
    private void InorderTraversal(TreeNode node)
    {
        if (node == null)
        {
            return;
        }
        InorderTraversal(node.left);
        inorder.Add(node.val);
        InorderTraversal(node.right);
    }

    /** @return the next smallest number */
    public int Next()
    {
        return inorder[index++];
    }

    /** @return whether we have a next smallest number */
    public bool HasNext()
    {
        return index < inorder.Count;
    }
}
```

## Approach 3: Morris Traversal (Space Optimized, Less Practical for Iterator)

### Solution
```csharp
// Time Complexity:
//   - Constructor: O(1) (no precomputation)
//   - Next(): O(1) on average across all calls
//   - HasNext(): O(1)
// Space Complexity: O(1), no additional storage
public class BSTIterator
{
    private TreeNode current;
    private TreeNode predecessor;

    public BSTIterator(TreeNode root)
    {
        current = root;
    }

    /** @return the next smallest number */
    public int Next()
    {
        int value = -1;

        while (current != null)
        {
            if (current.left == null)
            {
                value = current.val;
                current = current.right; // Move to the right subtree
                break;
            }
            else
            {
                predecessor = current.left;

                // Find the rightmost node in the left subtree
                while (predecessor.right != null && predecessor.right != current)
                {
                    predecessor = predecessor.right;
                }

                if (predecessor.right == null)
                {
                    predecessor.right = current; // Create a temporary link
                    current = current.left;
                }
                else
                {
                    predecessor.right = null; // Remove the temporary link
                    value = current.val;
                    current = current.right; // Move to the right subtree
                    break;
                }
            }
        }

        return value;
    }

    /** @return whether we have a next smallest number */
    public bool HasNext()
    {
        return current != null;
    }
}
```

