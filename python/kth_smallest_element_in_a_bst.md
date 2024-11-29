# 230. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

## Approach 1: Inorder Traversal (Recursive)

### Solution
```python
# Time Complexity: O(n), where n is the number of nodes in the tree (in the worst case)
# Space Complexity: O(h), where h is the height of the tree for the recursion stack
class Solution:
    def __init__(self):
        self.count = 0
        self.result = 0

    def kthSmallest(self, root: TreeNode, k: int) -> int:
        self.inorder(root, k)
        return self.result

    def inorder(self, node: TreeNode, k: int):
        if node is None:
            return
        
        self.inorder(node.left, k)  # Recurse for the left subtree
        
        self.count += 1  # Increment the count for each visited node
        if self.count == k:
            self.result = node.val  # Store the kth smallest value
            return
        
        self.inorder(node.right, k)  # Recurse for the right subtree
```

## Approach 2: Inorder Traversal (Iterative)

### Solution
```python
# Time Complexity: O(k), where k is the kth smallest element to be found
# Space Complexity: O(h), where h is the height of the tree for the stack
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        stack = []
        current = root
        count = 0

        while current is not None or stack:
            while current is not None:
                stack.append(current)  # Push nodes of the left subtree
                current = current.left

            current = stack.pop()  # Process the top node
            count += 1

            if count == k:
                return current.val  # Return the kth smallest value

            current = current.right  # Move to the right subtree

        return -1  # This line should never be reached if k is valid
```

## Approach 3: Binary Search on BST Property

### Solution
```python
# Time Complexity: O(h + k), where h is the height of the tree and k is the position of the kth smallest element
# Space Complexity: O(h), where h is the height of the tree for recursion stack
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        leftNodeCount = self.countNodes(root.left)

        if leftNodeCount == k - 1:
            return root.val  # Root is the kth smallest element
        elif leftNodeCount >= k:
            return self.kthSmallest(root.left, k)  # Recurse on the left subtree
        else:
            return self.kthSmallest(root.right, k - leftNodeCount - 1)  # Recurse on the right subtree

    def countNodes(self, node: TreeNode) -> int:
        if node is None:
            return 0
        return 1 + self.countNodes(node.left) + self.countNodes(node.right)  # Count nodes in the subtree
```


