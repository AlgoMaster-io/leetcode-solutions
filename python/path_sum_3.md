# 437. [Path Sum III](https://leetcode.com/problems/path-sum-iii/)

## Approach 1: Brute Force (DFS for Each Node)

### Solution
```python
# Time Complexity: O(n^2) in the worst case (skewed tree), O(n log n) for balanced tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack

class Solution:
    def pathSum(self, root: TreeNode, targetSum: int) -> int:
        if not root:
            return 0  # Base case: empty tree

        # Calculate paths including the current root and recurse for left and right subtrees
        return self.dfs(root, targetSum) + self.pathSum(root.left, targetSum) + self.pathSum(root.right, targetSum)

    def dfs(self, node: TreeNode, targetSum: int) -> int:
        if not node:
            return 0  # Base case: empty node

        count = 0
        if node.val == targetSum:
            count += 1  # Path ends at the current node

        # Check paths including left and right children
        count += self.dfs(node.left, targetSum - node.val)
        count += self.dfs(node.right, targetSum - node.val)

        return count
```

## Approach 2: Optimized Using Prefix Sum (HashMap)

### Solution
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h) for recursion stack and O(n) for HashMap storage

from collections import defaultdict

class Solution:
    def pathSum(self, root: TreeNode, targetSum: int) -> int:
        prefixSumMap = defaultdict(int)
        prefixSumMap[0] = 1  # Base case: one way to get sum 0
        return self.dfs(root, 0, targetSum, prefixSumMap)

    def dfs(self, node: TreeNode, currentSum: int, targetSum: int, prefixSumMap: dict) -> int:
        if not node:
            return 0  # Base case: empty node

        currentSum += node.val
        paths = prefixSumMap.get(currentSum - targetSum, 0)  # Count paths ending at this node

        # Update the prefix sum map for this node
        prefixSumMap[currentSum] = prefixSumMap.get(currentSum, 0) + 1

        # Recurse for left and right children
        paths += self.dfs(node.left, currentSum, targetSum, prefixSumMap)
        paths += self.dfs(node.right, currentSum, targetSum, prefixSumMap)

        # Backtrack: remove the current node's contribution to the prefix sum
        prefixSumMap[currentSum] -= 1

        return paths
```

