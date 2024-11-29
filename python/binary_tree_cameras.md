# 968. [Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approach 1: Greedy Approach with Recursion

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(h) where h is the height of the tree (recursion stack)
class Solution:
    def __init__(self):
        self.cameras = 0

    def minCameraCover(self, root: TreeNode) -> int:
        # If root returns 0, it means it needs a camera
        return self.dfs(root) == 0 and self.cameras + 1 or self.cameras

    def dfs(self, node: TreeNode) -> int:
        if not node:
            return 1  # This node is covered
        
        left = self.dfs(node.left)
        right = self.dfs(node.right)
        
        if left == 0 or right == 0:
            self.cameras += 1
            return 2  # Placing a camera at this node
        
        return 1 if left == 2 or right == 2 else 0
        # 2 indicates child has a camera, 1 means this node is covered
        # 0 indicates this node needs a camera

class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
```

## Approach 2: Dynamic Programming with State Tracking

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    HAS_CAMERA = 0
    COVERED = 1
    NEEDS_CAMERA = 2

    def minCameraCover(self, root: TreeNode) -> int:
        dp = {}

        def dfs(node: TreeNode) -> int:
            if not node:
                return self.COVERED

            if node not in dp:
                state = [None, None, None] 

                left = dfs(node.left)
                right = dfs(node.right)
                
                if left == self.NEEDS_CAMERA or right == self.NEEDS_CAMERA:
                    state[self.HAS_CAMERA] = dp.get(node, [0, 0, 0])[self.HAS_CAMERA] + 1
                    dp[node] = state
                    return self.HAS_CAMERA
                
                if left == self.HAS_CAMERA or right == self.HAS_CAMERA:
                    state[self.COVERED] = dp.get(node, [0, 0, 0])[self.COVERED]
                    dp[node] = state
                    return self.COVERED
                
                state[self.NEEDS_CAMERA] = dp.get(node, [0, 0, 0])[self.NEEDS_CAMERA]
                dp[node] = state
                return self.NEEDS_CAMERA
            
            return dp[node][0]
        
        return dfs(root) == self.NEEDS_CAMERA and dp[root][self.HAS_CAMERA] + 1 or dp[root][self.HAS_CAMERA]

class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
```

Note: The second approach is a detailed version using dynamic programming concepts, but the greedy recursive version is typically more straightforward and efficient due to its optimizations for this specific problem.

