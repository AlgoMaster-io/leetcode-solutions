# [Leetcode 669: Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

In this problem, you are given the root of a binary search tree and integer boundaries `low` and `high`, and you are required to trim the tree so that all its elements lie in `[low, high]`. You must return the root of the trimmed binary search tree.

## Approaches
- [Solution 1: Recursive Approach](#solution-1-recursive-approach)
- [Solution 2: Iterative Approach](#solution-2-iterative-approach)

### Solution 1: Recursive Approach
The recursive solution leverages the properties of a binary search tree to efficiently trim the tree. The idea is to recursively traverse the tree and adjust pointers as needed. Here's the step-by-step approach:

1. If the current node is `null`, return `null`.
2. If the current node's value is less than `low`, this means the entire left subtree of the current node is not needed. Thus, recursively trim the right subtree and return it.
3. If the current node's value is greater than `high`, the entire right subtree is not needed. Therefore, recursively trim the left subtree and return it.
4. Otherwise, recursively trim both the left and right subtrees and return the node itself.

```csharp
public TreeNode TrimBST(TreeNode root, int low, int high) {
    // Base Case: If node is null, simply return null
    if (root == null) return null;
    
    // If the current node's value is less than low, only the right subtree might have valid nodes
    if (root.val < low) {
        return TrimBST(root.right, low, high);
    }
    
    // If the current node's value is greater than high, only the left subtree might have valid nodes
    if (root.val > high) {
        return TrimBST(root.left, low, high);
    }
    
    // Recursively trim the left and right subtrees
    root.left = TrimBST(root.left, low, high);
    root.right = TrimBST(root.right, low, high);
    
    return root;
}
```

**Time Complexity**: O(n), where n is the number of nodes in the tree. We potentially visit each node once.
**Space Complexity**: O(h), where h is the height of the tree, representing the stack space for recursive calls.

### Solution 2: Iterative Approach
An iterative approach using a stack or a queue can also be devised, but for trimming a binary search tree, recursion is more natural and intuitive due to its divide-and-conquer nature. However, if you prefer an iterative method, you can use this following approach:

1. Use a stack to simulate the recursion. Push the root node onto the stack if it's non-null.
2. Pop nodes from the stack and perform similar checks as in the recursive approach:
   - If the node's value is less than `low`, add its right child to the stack.
   - If the node's value is greater than `high`, add its left child to the stack.
   - If the node is within `low` and `high`, adjust its left and right children using similar logic and add them to the stack.
3. Repeat until the stack is empty.

```csharp
public TreeNode TrimBST(TreeNode root, int low, int high) {
    if (root == null) return null;
    
    // Use a stack to process nodes
    Stack<TreeNode> stack = new Stack<TreeNode>();
    stack.Push(root);
    
    while (stack.Count > 0) {
        TreeNode node = stack.Pop();
        
        if (node != null) {
            // If the current node's value is less than low, discard the left subtree
            while (node != null && node.val < low) {
                node = node.right;
            }
            
            // If the current node's value is greater than high, discard the right subtree
            while (node != null && node.val > high) {
                node = node.left;
            }
            
            if (node != null) {
                if (node.left != null) stack.Push(node.left);
                if (node.right != null) stack.Push(node.right);
            }
        }
    }
    return root;
}
```

**Time Complexity**: O(n), as we iterate over nodes as in the recursive solution.
**Space Complexity**: O(h), due to the stack potentially storing all nodes on one side of the tree.

In general, the recursive solution is more concise and easier to understand, making it a preferred method for implementing tree-related problems like this one.

