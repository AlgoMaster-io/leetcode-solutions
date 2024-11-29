# 145. [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approach 1: Recursive Postorder Traversal

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack

class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        self.postorder(root, result)
        return result

    def postorder(self, node: TreeNode, result: List[int]):
        if not node:
            return # Base case: if the node is null, return

        self.postorder(node.left, result) # Recurse for the left subtree
        self.postorder(node.right, result) # Recurse for the right subtree
        result.append(node.val) # Visit the root
```

## Approach 2: Iterative Postorder Traversal Using Two Stacks

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(2n) = O(n), for two stacks

class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        if not root:
            return result

        stack1 = [root]
        stack2 = []

        while stack1:
            current = stack1.pop()
            stack2.append(current)

            if current.left:
                stack1.append(current.left) # Push left child to stack1
            if current.right:
                stack1.append(current.right) # Push right child to stack1

        while stack2:
            result.append(stack2.pop().val) # Add nodes in postorder from stack2

        return result
```

## Approach 3: Iterative Postorder Traversal Using One Stack

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for the stack

class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        if not root:
            return result

        stack = []
        current = root
        last_visited = None

        while current or stack:
            if current:
                stack.append(current) # Push nodes to the stack
                current = current.left # Move to the left subtree
            else:
                peek_node = stack[-1]
                if peek_node.right and last_visited != peek_node.right:
                    current = peek_node.right # Move to the right subtree
                else:
                    result.append(peek_node.val) # Visit the node
                    last_visited = stack.pop()

        return result
```

## Approach 4: Morris Postorder Traversal (Space Optimized)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(1), as no additional space is used

class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        dummy = TreeNode(0)
        dummy.left = root
        current, predecessor = dummy, None

        while current:
            if not current.left:
                current = current.right
            else:
                predecessor = current.left
                while predecessor.right and predecessor.right != current:
                    predecessor = predecessor.right

                if not predecessor.right:
                    predecessor.right = current # Create a temporary thread
                    current = current.left
                else:
                    self.reverseAddPath(current.left, predecessor, result)
                    predecessor.right = None # Remove the temporary thread
                    current = current.right

        return result

    def reverseAddPath(self, start: TreeNode, end: TreeNode, result: List[int]):
        temp = []
        while start != end:
            temp.append(start.val)
            start = start.right
        temp.append(end.val)
        for val in reversed(temp):
            result.append(val) # Add nodes in reverse order
```

