# 199. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

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
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        if not root:
            return result  # Return empty result if the tree is empty

        queue = deque([root])

        while queue:
            level_size = len(queue)
            last_node = None

            for _ in range(level_size):
                current_node = queue.popleft()  # Process the current node
                last_node = current_node

                if current_node.left:
                    queue.append(current_node.left)  # Add left child to the queue
                if current_node.right:
                    queue.append(current_node.right)  # Add right child to the queue

            result.append(last_node.val)  # Add the last node's value from this level

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
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        self.dfs(root, 0, result)
        return result

    def dfs(self, node: Optional[TreeNode], depth: int, result: List[int]):
        if not node:
            return  # Base case: if the node is null, return

        if depth == len(result):
            result.append(node.val)  # Add the first node of this depth (rightmost node)

        self.dfs(node.right, depth + 1, result)  # Visit the right subtree first
        self.dfs(node.left, depth + 1, result)  # Visit the left subtree next
```

