# 144. [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approach 1: Recursive Preorder Traversal

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack

class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        self.preorder(root, result)
        return result

    def preorder(self, node: TreeNode, result: List[int]) -> None:
        if node is None:
            return # Base case: if the node is null, return

        result.append(node.val) # Visit the root
        self.preorder(node.left, result) # Recurse for the left subtree
        self.preorder(node.right, result) # Recurse for the right subtree
```

## Approach 2: Iterative Preorder Traversal Using Stack

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for the stack

class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        if root is None:
            return result # Return empty result if the tree is empty

        stack = []
        stack.append(root)

        while stack:
            currentNode = stack.pop()
            result.append(currentNode.val) # Visit the root

            # Push right child first so that left child is processed first
            if currentNode.right is not None:
                stack.append(currentNode.right)
            if currentNode.left is not None:
                stack.append(currentNode.left)

        return result
```

## Approach 3: Morris Preorder Traversal

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(1), as no additional space is used

class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        current = root

        while current is not None:
            if current.left is None:
                result.append(current.val) # Visit the root
                current = current.right # Move to the right
            else:
                predecessor = current.left

                # Find the rightmost node in the left subtree
                while predecessor.right is not None and predecessor.right != current:
                    predecessor = predecessor.right

                if predecessor.right is None:
                    result.append(current.val) # Visit the root
                    predecessor.right = current # Create a temporary thread to the root
                    current = current.left # Move to the left
                else:
                    predecessor.right = None # Remove the temporary thread
                    current = current.right # Move to the right

        return result
```

