# 124. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approach: Depth-First Search (DFS)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack
class Solution:
    def __init__(self):
        self.max_sum = float('-inf')

    def maxPathSum(self, root):
        self.calculate_max_path(root)
        return self.max_sum

    def calculate_max_path(self, node):
        if node is None:
            return 0  # Base case: null node contributes 0 to the path sum

        # Recursively calculate the maximum path sum of the left and right subtrees
        left_max = max(0, self.calculate_max_path(node.left))  # Ignore negative sums
        right_max = max(0, self.calculate_max_path(node.right)) # Ignore negative sums

        # Update the global maximum path sum
        self.max_sum = max(self.max_sum, left_max + right_max + node.val)

        # Return the maximum path sum including the current node and one of its subtrees
        return max(left_max, right_max) + node.val
```


