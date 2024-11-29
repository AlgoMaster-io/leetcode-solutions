# 103. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approach 1: Breadth-First Search with Direction Toggle

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(n) for the queue and result storage
from collections import deque

class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        result = []
        if not root:
            return result  # Return empty result if the tree is empty

        queue = deque([root])
        left_to_right = True

        while queue:
            level_size = len(queue)
            current_level = deque()

            for _ in range(level_size):
                current_node = queue.popleft()  # Process the current node

                if left_to_right:
                    current_level.append(current_node.val)  # Add at the end
                else:
                    current_level.appendleft(current_node.val)  # Add at the beginning

                if current_node.left:
                    queue.append(current_node.left)  # Add left child to the queue
                if current_node.right:
                    queue.append(current_node.right)  # Add right child to the queue

            result.append(list(current_level))  # Add the current level to the result
            left_to_right = not left_to_right  # Toggle the direction for the next level

        return result
```

## Approach 2: Depth-First Search (Recursive with Level Tracking)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack

class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        result = []
        self.dfs(root, 0, result)
        return result

    def dfs(self, node: TreeNode, level: int, result: List[List[int]]):
        if not node:
            return  # Base case: if the node is null, return

        if level == len(result):
            result.append(deque())  # Create a new level in the result

        if level % 2 == 0:
            result[level].append(node.val)  # Add normally for even levels
        else:
            result[level].appendleft(node.val)  # Add to the front for odd levels

        self.dfs(node.left, level + 1, result)  # Recurse for the left child
        self.dfs(node.right, level + 1, result)  # Recurse for the right child
```

