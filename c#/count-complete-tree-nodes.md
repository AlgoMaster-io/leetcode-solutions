# [Leetcode Problem 222: Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

## Navigation
- [Approach 1: Basic Recursive Traversal](#approach-1-basic-recursive-traversal)
- [Approach 2: Iterative Level Order Traversal](#approach-2-iterative-level-order-traversal)
- [Approach 3: Binary Search on Depth](#approach-3-binary-search-on-depth)

## Approach 1: Basic Recursive Traversal

### Intuition
The simplest approach to count the nodes in a complete binary tree is to traverse through every node recursively and count each one. This can be achieved using a depth-first search (DFS) strategy. While this method is easy to implement, it doesn't take full advantage of the properties of a complete binary tree and may be less efficient.

### Code
```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public int CountNodes(TreeNode root) {
        if (root == null) return 0;
        // Recursively count nodes in the left and right subtrees and add 1 for the root.
        return 1 + CountNodes(root.left) + CountNodes(root.right);
    }
}
```

### Time Complexity
- **O(n)**: In the worst case, we visit each node once where `n` is the number of nodes.

### Space Complexity
- **O(h)**: The space complexity is the height of the tree due to the function call stack in recursion.

---

## Approach 2: Iterative Level Order Traversal

### Intuition
An iteration-based approach can also be used by employing a level order traversal (BFS). We'll utilize a queue to perform the traversal and count nodes level by level. This method is straightforward and avoids the recursive call stack, but like the first approach, it does not capitalize on the completeness nature of the binary tree.

### Code
```csharp
using System.Collections.Generic;

public class Solution {
    public int CountNodes(TreeNode root) {
        if (root == null) return 0;
        int count = 0;
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);
        
        while (queue.Count > 0) {
            TreeNode node = queue.Dequeue();
            count++;
            // Add left and right children of the current node to the queue
            if (node.left != null) queue.Enqueue(node.left);
            if (node.right != null) queue.Enqueue(node.right);
        }
        
        return count;
    }
}
```

### Time Complexity
- **O(n)**: Each node of the tree is visited once.

### Space Complexity
- **O(n)**: In the worst case, the queue stores all nodes in one level, which could be as many as `n/2` nodes.

---

## Approach 3: Binary Search on Depth

### Intuition
A complete binary tree is perfectly balanced except for the last level, which is filled from left to right. We can exploit this property using a binary search approach to efficiently calculate the number of nodes. By determining the depth of the left and right subtrees, we can directly compute the number of nodes present in perfect subtrees, thus avoiding the need to traverse every node.

### Code
```csharp
public class Solution {
    public int CountNodes(TreeNode root) {
        if (root == null) return 0;
        
        int leftDepth = GetDepth(root.left);
        int rightDepth = GetDepth(root.right);
        
        // If left and right subtree have the same depth, the left subtree is perfect
        if (leftDepth == rightDepth) {
            // left subtree is perfect and right subtree is complete
            return (1 << leftDepth) + CountNodes(root.right);
        } else {
            // right subtree is perfect and left subtree is complete
            return (1 << rightDepth) + CountNodes(root.left);
        }
    }
    
    // Helper function to compute the depth of the leftmost path
    private int GetDepth(TreeNode node) {
        int depth = 0;
        while (node != null) {
            depth++;
            node = node.left; // go down along the leftmost edge
        }
        return depth;
    }
}
```

### Time Complexity
- **O((log n)^2)**: Each level reduces the problem size significantly and computing depth itself takes **O(log n)**.

### Space Complexity
- **O(log n)**: The space complexity is due to the recursive stack depth, which is logarithmic in the number of nodes for a balanced tree. 

By implementing the above varying approaches, you can efficiently solve the problem of counting nodes in a complete binary tree, choosing the approach based on your specific constraints and needs.

