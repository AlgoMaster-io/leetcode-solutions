# 337. [House Robber III](https://leetcode.com/problems/house-robber-iii/)

## Approach 1: Recursive Approach with Memoization

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def __init__(self):
        self.memo = {}

    def rob(self, root: TreeNode) -> int:
        if not root:
            return 0
        if root in self.memo:
            return self.memo[root]

        # if we rob this root, we cannot rob its direct children
        robRoot = root.val
        if root.left:
            robRoot += self.rob(root.left.left) + self.rob(root.left.right)
        if root.right:
            robRoot += self.rob(root.right.left) + self.rob(root.right.right)

        # if we do not rob this root, we are free to rob its children
        skipRoot = self.rob(root.left) + self.rob(root.right)

        # Choose the maximum money we can rob
        result = max(robRoot, skipRoot)
        self.memo[root] = result  # Memoize the result for current node

        return result
```

## Approach 2: Dynamic Programming with Tree DP

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(h), where h is the height of the tree
class Solution:
    def rob(self, root: TreeNode) -> int:
        result = self.robSub(root)
        return max(result[0], result[1])

    def robSub(self, root: TreeNode) -> [int, int]:
        if not root:
            return [0, 0]  # Entry for {not rob, rob}

        left = self.robSub(root.left)
        right = self.robSub(root.right)

        # if we don't rob this node, we can choose to rob or not to rob its children
        notRob = max(left[0], left[1]) + max(right[0], right[1])

        # if we rob this node, we cannot rob its children
        rob = root.val + left[0] + right[0]

        return [notRob, rob]  # Return array containing {not rob, rob}
```

