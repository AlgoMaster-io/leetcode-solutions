# 94. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approach 1: Recursive Inorder Traversal

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack

class Solution:
    def inorderTraversal(self, root):
        result = []
        self.inorder(root, result)
        return result

    def inorder(self, node, result):
        if not node:
            return  # Base case: if the node is null, return

        self.inorder(node.left, result)  # Recurse for the left subtree
        result.append(node.val)  # Visit the root
        self.inorder(node.right, result)  # Recurse for the right subtree
```

## Approach 2: Iterative Inorder Traversal Using Stack

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for the stack

class Solution:
    def inorderTraversal(self, root):
        result, stack = [], []
        current = root

        while current or stack:
            while current:
                stack.append(current)  # Push the current node and move to the left subtree
                current = current.left

            current = stack.pop()  # Process the top node
            result.append(current.val)  # Visit the node
            current = current.right  # Move to the right subtree

        return result
```

## Approach 3: Morris Inorder Traversal (Without Recursion or Stack)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(1), as no additional space is used

class Solution:
    def inorderTraversal(self, root):
        result = []
        current = root

        while current:
            if not current.left:
                result.append(current.val)  # Visit the node
                current = current.right  # Move to the right subtree
            else:
                predecessor = current.left

                # Find the rightmost node in the left subtree
                while predecessor.right and predecessor.right != current:
                    predecessor = predecessor.right

                if not predecessor.right:
                    predecessor.right = current  # Create a temporary thread to the root
                    current = current.left  # Move to the left subtree
                else:
                    predecessor.right = None  # Remove the temporary thread
                    result.append(current.val)  # Visit the node
                    current = current.right  # Move to the right subtree

        return result
```


