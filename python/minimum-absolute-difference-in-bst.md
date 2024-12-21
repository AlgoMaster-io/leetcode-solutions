# [Leetcode 530: Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

### Approaches:
- [Approach 1: Inorder Traversal (Iterative)](#approach-1-inorder-traversal-iterative)
- [Approach 2: Inorder Traversal (Recursive)](#approach-2-inorder-traversal-recursive)

## Approach 1: Inorder Traversal (Iterative)

### Intuition
The problem asks us to find the minimum absolute difference between values of any two nodes in a Binary Search Tree (BST). In a BST, an inorder traversal gives nodes in a sorted order. By leveraging this property, if we traverse the tree in inorder fashion, the minimum absolute difference will be between any two consecutive nodes.

### Steps
1. Use an iterative inorder traversal to visit nodes and keep track of the previous node value.
2. Calculate the difference between the current node's value and the previous node's value in the inorder traversal.
3. Update the minimum difference found so far.
4. Continue until all nodes have been visited.

### Code

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def getMinimumDifference(root: TreeNode) -> int:
    # Base condition
    if not root:
        return 0

    # Initialize a stack for iterative traversal
    stack = []
    current = root
    prev = None
    min_diff = float('inf')

    # Inorder traversal loop
    while stack or current:
        while current:  # Reach the leftmost node of the current Node
            stack.append(current)
            current = current.left

        # Current must be None at this point
        current = stack.pop()
        
        # If there's a previous node, check the difference
        if prev is not None:
            min_diff = min(min_diff, current.val - prev)

        # Update previous node value as current node's value
        prev = current.val

        # Visit the right subtree
        current = current.right

    return min_diff
```

### Time Complexity
- O(N), where N is the number of nodes in the BST. We need to visit each node exactly once.

### Space Complexity
- O(H), where H is the height of the tree. The space is used by the stack for the largest depth times (in skewed trees).

## Approach 2: Inorder Traversal (Recursive)

### Intuition
Similar to the iterative approach, but here we utilize the recursive method for inorder traversal. This makes the implementation simpler by handling the traversal via the function call stack.

### Steps
1. Perform a recursive inorder traversal.
2. Keep track of the previous node using a shared variable.
3. Evaluate the minimum difference at each step.
4. At the end of the traversal, the minimum difference will be captured.

### Code

```python
def getMinimumDifference(root: TreeNode) -> int:
    # Shared state across recursion
    prev = [-1]  # Use a list to allow mutation
    min_diff = [float('inf')]

    def inorder(node):
        if not node:
            return
            
        # Inorder traversal: Left
        inorder(node.left)
        
        # Process current node
        if prev[0] != -1:
            min_diff[0] = min(min_diff[0], node.val - prev[0])
            
        # Update the previous node value
        prev[0] = node.val
        
        # Inorder traversal: Right
        inorder(node.right)
        
    inorder(root)
    return min_diff[0]
```

### Time Complexity
- O(N), where N is the number of nodes in the BST. Similar to the previous approach.

### Space Complexity
- O(H), where H is the height of the tree due to the recursion stack.

