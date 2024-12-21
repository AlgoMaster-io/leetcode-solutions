# [Leetcode 144: Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach Using Stack](#iterative-approach-using-stack)

## Recursive Approach

### Intuition
In preorder traversal, we visit nodes in the order: root -> left -> right. This can be naturally implemented using recursion since it aligns perfectly with the recursive definition of a tree (every subtree is itself a tree).

### Approach
1. Start from the root node.
2. Visit the root node.
3. Recursively traverse the left subtree.
4. Recursively traverse the right subtree.
5. Concatenate the results in the order of visitation.

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def preorderTraversal(root: TreeNode):
    def helper(node, result):
        if node is None:
            return
        # Visit the root
        result.append(node.val)
        # Traverse the left subtree
        helper(node.left, result)
        # Traverse the right subtree
        helper(node.right, result)
    
    result = []
    helper(root, result)
    return result
```

### Time Complexity
- **O(n)** where n is the number of nodes in the tree, since each node is visited exactly once.

### Space Complexity
- **O(n)** for the call stack in the worst case (skewed tree).

## Iterative Approach Using Stack

### Intuition
An iterative approach to implementing preorder traversal can be achieved using a stack which simulates the call stack of our recursive solution. The idea is to replicate the root-left-right visiting sequence using a stack.

### Approach
1. Initialize an empty stack and push the root into it if it's not null.
2. While the stack is not empty:
   - Pop an element from the stack and visit it.
   - Push its right child into the stack if it exists (we push right first because stack is LIFO).
   - Push its left child into the stack if it exists.
3. The traversal order is ensured by the order of push operations (right, then left).

```python
def preorderTraversal(root: TreeNode):
    if root is None:
        return []
    
    stack, result = [root], []
    while stack:
        node = stack.pop()
        # Visit the root
        result.append(node.val)
        # Push right child to stack first, so that the left is processed first
        if node.right:
            stack.append(node.right)
        # Push left child to stack
        if node.left:
            stack.append(node.left)
    
    return result
```

### Time Complexity
- **O(n)** where n is the number of nodes, since each node is visited exactly once.

### Space Complexity
- **O(n)** in the worst case due to the stack space used, in cases of skewed trees (deep stacks). However, average case would be less for balanced trees.

In both approaches, the fundamental idea is based on pre-order nature: visit, then traverse left, and then traverse right. Each method provides a way to achieve this traversal with varying implications on space due to the choice between system (recursive) or manual stack (iterative).

