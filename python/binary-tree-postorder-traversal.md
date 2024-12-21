# [Leetcode 145: Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Table of Contents
- [Approach 1: Recursive Postorder Traversal](#approach-1)
- [Approach 2: Iterative Postorder Traversal using Two Stacks](#approach-2)
- [Approach 3: Iterative Postorder Traversal using One Stack](#approach-3)

## Approach 1: Recursive Postorder Traversal

The simplest way to perform a postorder traversal of a binary tree is through a recursive method. In postorder traversal, nodes are recursively visited in the order: left subtree, right subtree, and then the root node.

### Intuition:
- Recursion provides a straightforward way to implement DFS-like traversals such as postorder.
- By exploring the left subtree first, followed by the right, and then the root, we naturally follow the postorder sequence.

### Python Code:
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def postorderTraversal(root: TreeNode) -> List[int]:
    def helper(node, output):
        if node is None:
            return
        # Traverse the left subtree
        helper(node.left, output)
        # Traverse the right subtree
        helper(node.right, output)
        # Visit the root node
        output.append(node.val)

    result = []
    helper(root, result)
    
    return result
```

### Complexity Analysis:
- **Time Complexity:** O(n), where n is the number of nodes in the tree, as each node is visited once.
- **Space Complexity:** O(h), where h is the height of the tree, for the recursion stack.

## Approach 2: Iterative Postorder Traversal using Two Stacks

An iterative approach can be implemented using two stacks. The idea is to modify a preorder traversal (root, left, right) into a postorder (left, right, root) by using an additional stack.

### Intuition:
- Utilize Stack 1 to perform a modified preorder traversal (root, right, left).
- Use Stack 2 to reverse the result to achieve the postorder sequence (left, right, root).

### Python Code:
```python
def postorderTraversal(root: TreeNode) -> List[int]:
    if root is None:
        return []
    
    stack1, stack2 = [], []
    stack1.append(root)
    
    while stack1:
        node = stack1.pop()
        stack2.append(node)
        # Add left and then right child to stack1
        if node.left:
            stack1.append(node.left)
        if node.right:
            stack1.append(node.right)
    
    # Pop all items from stack2, which will be in postorder sequence
    result = []
    while stack2:
        result.append(stack2.pop().val)
    
    return result
```

### Complexity Analysis:
- **Time Complexity:** O(n), where n is the number of nodes, as each node is pushed and popped from the stacks once.
- **Space Complexity:** O(n), where n is the number of nodes, due to the storage of nodes in the stacks.

## Approach 3: Iterative Postorder Traversal using One Stack

We can further optimize the iterative approach by using only one stack. It involves keeping track of the previously visited node to determine whether nodes should be processed or collected.

### Intuition:
- Navigate to the leftmost node, collecting nodes in the process.
- Track the last visited node to make an informed decision about moving back up the tree.
- If a right subtree exists and is traversed, mark it in the logic to dictate traversal direction.

### Python Code:
```python
def postorderTraversal(root: TreeNode) -> List[int]:
    result = []
    stack = []
    last_visited = None
    current = root

    while current or stack:
        while current:
            stack.append(current)
            current = current.left

        current = stack[-1]
        # If right child is None or already visited, visit the node
        if current.right is None or current.right == last_visited:
            result.append(current.val)
            stack.pop()
            last_visited = current
            current = None
        else:
            # Traverse to the right child
            current = current.right

    return result
```

### Complexity Analysis:
- **Time Complexity:** O(n), where n is the number of nodes, as each node is visited once.
- **Space Complexity:** O(h), where h is the height of the tree, for the stack space.

