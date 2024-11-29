# 783. [Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Approach 1: Inorder Traversal with a List

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(n) for storing the inorder traversal

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def minDiffInBST(self, root: TreeNode) -> int:
        values = []
        self.inorder(root, values)

        minDiff = float('inf')
        for i in range(1, len(values)):
            minDiff = min(minDiff, values[i] - values[i - 1])

        return minDiff

    def inorder(self, node: TreeNode, values: list):
        if node is None:
            return

        self.inorder(node.left, values)  # Recurse for the left subtree
        values.append(node.val)          # Add the current node value
        self.inorder(node.right, values) # Recurse for the right subtree
```

## Approach 2: Inorder Traversal with Constant Space

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for the recursion stack

class Solution:
    def __init__(self):
        self.prev = None
        self.minDiff = float('inf')

    def minDiffInBST(self, root: TreeNode) -> int:
        self.inorder(root)
        return self.minDiff

    def inorder(self, node: TreeNode):
        if node is None:
            return

        self.inorder(node.left)  # Recurse for the left subtree

        if self.prev is not None:
            self.minDiff = min(self.minDiff, node.val - self.prev)  # Calculate the difference with the previous value
        self.prev = node.val  # Update the previous node value

        self.inorder(node.right)  # Recurse for the right subtree
```

## Approach 3: Morris Traversal (Space Optimized)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(1), as no additional space is used

class Solution:
    def minDiffInBST(self, root: TreeNode) -> int:
        current = root
        prev = None
        minDiff = float('inf')

        while current is not None:
            if current.left is None:
                # Process the current node
                if prev is not None:
                    minDiff = min(minDiff, current.val - prev.val)
                prev = current
                current = current.right  # Move to the right subtree
            else:
                predecessor = current.left

                # Find the rightmost node in the left subtree
                while predecessor.right is not None and predecessor.right != current:
                    predecessor = predecessor.right

                if predecessor.right is None:
                    predecessor.right = current  # Create a temporary thread to the root
                    current = current.left  # Move to the left subtree
                else:
                    predecessor.right = None  # Remove the temporary thread
                    if prev is not None:
                        minDiff = min(minDiff, current.val - prev.val)
                    prev = current
                    current = current.right  # Move to the right subtree

        return minDiff
```

