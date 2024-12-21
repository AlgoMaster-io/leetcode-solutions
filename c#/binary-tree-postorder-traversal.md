# [Leetcode 145: Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approaches:
- [Recursive Postorder Traversal](#recursive-postorder-traversal)
- [Iterative Postorder Traversal Using Two Stacks](#iterative-postorder-traversal-using-two-stacks)
- [Iterative Postorder Traversal Using One Stack](#iterative-postorder-traversal-using-one-stack)

### Recursive Postorder Traversal

The postorder traversal of a binary tree involves visiting the left subtree, then the right subtree, and finally the root node. A simple way to achieve this is by using recursion, which closely mirrors the definition of postorder traversal:

```csharp
public IList<int> PostorderTraversal(TreeNode root) {
    List<int> result = new List<int>();
    Postorder(root, result);
    return result;
}

private void Postorder(TreeNode node, List<int> result) {
    if (node == null) {
        return;
    }
    // Traverse the left subtree
    Postorder(node.left, result);
    // Traverse the right subtree
    Postorder(node.right, result);
    // Visit the root node
    result.Add(node.val);
}
```

**Intuition:**
- The recursive approach is straightforward - traverse left subtree, right subtree, then process the current node.
- The recursion stack in the function implicitly maintains a call stack, so no additional data structure is needed.

**Time Complexity:** O(n) - each node is visited once.
**Space Complexity:** O(n) - in the worst-case, the depth of the recursion stack can be `n` for a skewed tree.

### Iterative Postorder Traversal Using Two Stacks

An iterative approach can be achieved using two stacks. The idea is to push the nodes onto the first stack similar to preorder traversal (node, right, left), and then pop nodes from the first stack to push onto the second stack. Finally, popping from the second stack gives us the postorder sequence.

```csharp
public IList<int> PostorderTraversal(TreeNode root) {
    List<int> result = new List<int>();
    if (root == null) return result;

    Stack<TreeNode> stack1 = new Stack<TreeNode>();
    Stack<TreeNode> stack2 = new Stack<TreeNode>();
    
    stack1.Push(root);
    while (stack1.Count > 0) {
        TreeNode node = stack1.Pop();
        stack2.Push(node);
        if (node.left != null) {
            stack1.Push(node.left);
        }
        if (node.right != null) {
            stack1.Push(node.right);
        }
    }
    while (stack2.Count > 0) {
        result.Add(stack2.Pop().val);
    }
    return result;
}
```

**Intuition:**
- First stack processes nodes in a modified preorder sequence.
- Second stack holds nodes in reverse postorder sequence; popping gives correct postorder.

**Time Complexity:** O(n) - each node is visited once.
**Space Complexity:** O(n) - stacks can hold all nodes in the worst-case.

### Iterative Postorder Traversal Using One Stack

Using one stack, an additional pointer (`prev`) helps in determining whether to process or revisit nodes. This approach manually controls the traversal without using recursion.

```csharp
public IList<int> PostorderTraversal(TreeNode root) {
    List<int> result = new List<int>();
    if (root == null) return result;

    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode prev = null;
    
    while (root != null || stack.Count > 0) {
        while (root != null) {
            stack.Push(root);
            root = root.left; // Traverse to the leftmost node
        }
        
        root = stack.Peek(); // Peek at the node on top of the stack
        // If the right node is null, or right node was previously visited
        if (root.right == null || root.right == prev) {
            result.Add(root.val); // Visit the current node
            prev = root;
            stack.Pop(); // Remove the node from the stack
            root = null; // Set root to null to control while loop
        } else {
            root = root.right; // Process right subtree if not visited
        }
    }
    
    return result;
}
```

**Intuition:**
- Use a stack to simulate the control flow of recursive postorder traversal.
- Track previously processed nodes to handle backtracking correctly.

**Time Complexity:** O(n) - each node is visited once.
**Space Complexity:** O(n) - stack can be as large as the number of nodes in the worst-case.

