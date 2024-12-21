
# [LeetCode 173: Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approaches

- [Approach 1: Inorder Traversal with List Storage](#approach-1-inorder-traversal-with-list-storage)
- [Approach 2: Controlled Inorder Traversal using Stack (Optimal)](#approach-2-controlled-inorder-traversal-using-stack-optimal)

## Approach 1: Inorder Traversal with List Storage

### Intuition
The simplest way to implement a BST iterator is to perform an inorder traversal of the entire tree at the start and store the results in a list. This makes the `next` operation straightforward as it just involves fetching the next element from the list. However, the trade-off is significant space usage, especially for large trees.

### Code

```csharp
public class BSTIterator {
    private List<int> nodesSorted;
    private int index;

    public BSTIterator(TreeNode root) {
        nodesSorted = new List<int>();
        index = -1;
        // Fill the list with the inorder traversal of the tree
        InorderTraversal(root);
    }

    private void InorderTraversal(TreeNode root) {
        if (root == null) return;
        
        InorderTraversal(root.left); // Visit the left subtree
        nodesSorted.Add(root.val);   // Visit the node itself
        InorderTraversal(root.right); // Visit the right subtree
    }
    
    /** @return the next smallest number */
    public int Next() {
        return nodesSorted[++index];
    }
    
    /** @return whether we have a next smallest number */
    public bool HasNext() {
        return index + 1 < nodesSorted.Count;
    }
}
```

### Time Complexity
- Constructor: O(n), where n is the number of nodes in the tree, due to inorder traversal.
- `next()`: O(1), just returns the next element from the list.
- `hasNext()`: O(1), simple comparison.

### Space Complexity
- O(n), where n is the number of nodes in the tree, to store the entire inorder traversal.


## Approach 2: Controlled Inorder Traversal using Stack (Optimal)

### Intuition
Instead of storing all nodes, we maintain a controlled traversal using a stack. This stack holds nodes that have been visited but whose right child and descendants are yet to be explored. This solution iteratively processes nodes to always yield the next smallest node immediately without storing all of them beforehand.

### Code

```csharp
public class BSTIterator {
    private Stack<TreeNode> stack;

    public BSTIterator(TreeNode root) {
        stack = new Stack<TreeNode>();
        // Initialize the stack with the root and all its left descendants
        PushAllLeft(root);
    }

    private void PushAllLeft(TreeNode node) {
        while (node != null) {
            stack.Push(node);
            node = node.left; // Go to the left child
        }
    }
    
    /** @return the next smallest number */
    public int Next() {
        // The topmost node on the stack is the next smallest
        TreeNode nextNode = stack.Pop();
        // If the node has a right child, push all of its left descendants
        if (nextNode.right != null) {
            PushAllLeft(nextNode.right);
        }
        return nextNode.val;
    }
    
    /** @return whether we have a next smallest number */
    public bool HasNext() {
        return stack.Count > 0;
    }
}
```

### Time Complexity
- Constructor: O(h), where h is the height of the tree, due to stack initialization with all left nodes.
- `Next()`: Amortized O(1), each node is pushed and popped once.
- `HasNext()`: O(1), just checks stack count.

### Space Complexity
- O(h), where h is the height of the tree due to the stack usage for left subtree nodes.

This controlled traversal approach edges the trade-off between time and space, efficiently handling potentially large trees.

