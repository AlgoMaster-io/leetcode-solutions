# [Leetcode 98: Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Solutions
- [Inorder Traversal Recursion Approach](#inorder-traversal-recursion-approach)
- [Inorder Traversal Iterative Approach](#inorder-traversal-iterative-approach)
- [Recursive Range Checking Approach](#recursive-range-checking-approach)

### Inorder Traversal Recursion Approach

#### Intuition
The binary search tree (BST) property involves all left children being less than their parent node and all right children being greater than their parent node. Performing an inorder traversal of a BST yields a sorted list of values, i.e., a non-decreasing order. If the inorder traversal of a given tree is sorted, then the tree is a BST.

#### Implementation
The idea is to perform an inorder traversal and check if the sequence of visited values is strictly increasing.

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
    private int? lastValue = null;

    public bool IsValidBST(TreeNode root) {
        return InorderTraversal(root);
    }
    
    private bool InorderTraversal(TreeNode node) {
        if (node == null) return true;

        // Check the left subtree
        if (!InorderTraversal(node.left)) return false;
        
        // Check current node and ensure it's greater than the last seen value
        if (lastValue.HasValue && node.val <= lastValue.Value) return false;
        
        // Update last value with the current node's value
        lastValue = node.val;

        // Check the right subtree
        return InorderTraversal(node.right);
    }
}
```

#### Time Complexity
- **O(n)**: We traverse every node once.

#### Space Complexity
- **O(n)**: In the worst case, the space is used up by the recursion stack for a completely unbalanced tree which can go up to `n` calls deep.

---

### Inorder Traversal Iterative Approach

#### Intuition
Utilize a stack to mimic the recursive inorder traversal algorithm iteratively. This helps in checking if the tree is a valid BST using the iterative approach, which follows the same logic as the recursive inorder method.

#### Implementation
Use an explicit stack to perform inorder traversal.

```csharp
public class Solution {
    public bool IsValidBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        int? lastValue = null;
        
        while (root != null || stack.Count > 0) {
            while (root != null) {
                // Push all left children to the stack
                stack.Push(root);
                root = root.left;
            }
            
            root = stack.Pop();  // Visit the node
            
            // Check if current node's value is in order
            if (lastValue.HasValue && root.val <= lastValue.Value) return false;
            lastValue = root.val;
            
            root = root.right;  // Shift to right child
        }
        
        return true;
    }
}
```

#### Time Complexity
- **O(n)**: Every node is pushed and popped from the stack once.

#### Space Complexity
- **O(n)**: Stack could grow as large as the height of the tree, which in the worst case could be `n` nodes for unbalanced trees.

---

### Recursive Range Checking Approach

#### Intuition
By using recursion, maintain a range for each node that it must lie within for the tree to be considered a valid BST. The range is updated and checked for every node ensuring all nodes satisfy the BST conditions.

#### Implementation
For each node, ensure its value is more than the allowed lower limit and less than the allowed upper limit resulting from its ancestors.

```csharp
public class Solution {
    public bool IsValidBST(TreeNode root) {
        return Validate(root, null, null);
    }
    
    private bool Validate(TreeNode node, int? lower, int? upper) {
        if (node == null) return true;
        
        // Check node value is within the defined range
        if ((lower.HasValue && node.val <= lower.Value) || 
            (upper.HasValue && node.val >= upper.Value)) return false;

        // Recursively validate left and right subtree with updated range
        if (!Validate(node.left, lower, node.val)) return false;
        if (!Validate(node.right, node.val, upper)) return false;
        
        return true;
    }
}
```

#### Time Complexity
- **O(n)**: Each node is visited once.

#### Space Complexity
- **O(n)**: Space used by the recursion stack in the worst-case unbalanced tree.

