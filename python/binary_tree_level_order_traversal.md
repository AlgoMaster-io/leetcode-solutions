# 102. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## Approach 1: Breadth-First Search (Using Queue)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(n) for the queue and result storage
from collections import deque
from typing import List, Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        result = []
        if not root:
            return result # Return empty result if the tree is empty

        queue = deque([root])

        while queue:
            levelSize = len(queue)
            currentLevel = []

            for _ in range(levelSize):
                currentNode = queue.popleft() # Process the current node
                currentLevel.append(currentNode.val)

                if currentNode.left:
                    queue.append(currentNode.left) # Add left child to the queue
                if currentNode.right:
                    queue.append(currentNode.right) # Add right child to the queue

            result.append(currentLevel) # Add the current level to the result

        return result
```

## Approach 2: Depth-First Search (Recursive)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack
from typing import List, Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        result = []
        self.dfs(root, 0, result)
        return result

    def dfs(self, node: Optional[TreeNode], level: int, result: List[List[int]]):
        if not node:
            return # Base case: if the node is null, return

        if level == len(result):
            result.append([]) # Create a new level in the result

        result[level].append(node.val) # Add the current node's value to the current level

        self.dfs(node.left, level + 1, result) # Recurse for the left child
        self.dfs(node.right, level + 1, result) # Recurse for the right child
```

