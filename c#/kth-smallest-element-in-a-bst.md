# [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

## Approaches

1. [Inorder Traversal (Recursive)](#inorder-traversal-recursive)
2. [Inorder Traversal (Iterative)](#inorder-traversal-iterative)

### Inorder Traversal (Recursive)

The intuition behind this approach is that in a Binary Search Tree (BST), an in-order traversal (left-root-right) will yield the values in sorted order. By keeping a count of visited nodes during the traversal, we can stop as soon as we reach the kth node.

#### Code

```csharp
public class Solution {
    private int count = 0;
    private int result = 0;
    
    public int KthSmallest(TreeNode root, int k) {
        Inorder(root, k);
        return result;
    }
    
    private void Inorder(TreeNode root, int k) {
        if (root == null) return;
        
        // Traverse the left subtree
        Inorder(root.left, k);
        
        // Increment counter when visiting each node
        count++;
        // Check if current node is the kth node
        if (count == k) {
            result = root.val;
            return;
        }
        
        // Traverse the right subtree
        Inorder(root.right, k);
    }
}
```

#### Time and Space Complexity

- **Time Complexity**: O(N), where N is the number of nodes in the BST. In the worst case, we might have to visit all nodes.
- **Space Complexity**: O(H), where H is the height of the tree, representing the call stack due to recursion.

### Inorder Traversal (Iterative)

Alternative to recursion, we can simulate in-order traversal using a stack. This approach iteratively traverses to the leftmost node, processes nodes, and then traverses the right subtree.

#### Code

```csharp
public class Solution {
    public int KthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode current = root;
        
        // Continue until there are unvisited nodes or the kth node is found
        while (current != null || stack.Count > 0) {
            // Go to the leftmost node
            while (current != null) {
                stack.Push(current);
                current = current.left;
            }
            
            // Process the leftmost node
            current = stack.Pop();
            k--;
            // If kth node is found, return its value
            if (k == 0) return current.val;
            
            // Proceed to the right subtree
            current = current.right;
        }
        
        return -1; // This will never be reached if the tree has at least k nodes
    }
}
```

#### Time and Space Complexity

- **Time Complexity**: O(N), where N is the number of nodes in the BST. We might visit every node until the kth smallest is found.
- **Space Complexity**: O(H), where H is the height of the tree, attributed to the stack used in the iterative approach.

