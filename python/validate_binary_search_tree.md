# 98. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Approach 1: Inorder Traversal (Iterative)

### Solution
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for the stack
class Solution:
    def isValidBST(self, root):
        stack = []
        current = root
        prev = None

        while current is not None or stack:
            while current is not None:
                stack.append(current)  # Push nodes of the left subtree
                current = current.left
            
            current = stack.pop()  # Process the top node

            if prev is not None and current.val <= prev.val:
                return False  # If the current value is not greater than the previous, it's invalid
            prev = current

            current = current.right  # Move to the right subtree

        return True
```

## Approach 2: Recursive Inorder Traversal

### Solution
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for the recursion stack
class Solution:
    def __init__(self):
        self.prev = None
    
    def isValidBST(self, root):
        return self.inorder(root)
    
    def inorder(self, node):
        if node is None:
            return True  # Base case: empty tree is valid
        
        if not self.inorder(node.left):
            return False  # Left subtree is invalid
        
        if self.prev is not None and node.val <= self.prev.val:
            return False  # Current value is not greater than the previous
        self.prev = node  # Update the previous node

        return self.inorder(node.right)  # Recurse for the right subtree
```

## Approach 3: DFS with Range Validation

### Solution
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for the recursion stack
class Solution:
    def isValidBST(self, root):
        return self.validate(root, None, None)

    def validate(self, node, lower, upper):
        if node is None:
            return True  # Base case: empty tree is valid
        
        if (lower is not None and node.val <= lower) or (upper is not None and node.val >= upper):
            return False  # Node value violates the range constraints

        # Recursively validate the left and right subtrees
        return (self.validate(node.left, lower, node.val) and
                self.validate(node.right, node.val, upper))
```

