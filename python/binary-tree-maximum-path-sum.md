# [Leetcode 124: Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approaches:
1. [Recursive Depth-First Search (DFS)](#recursive-depth-first-search-dfs)

### Recursive Depth-First Search (DFS)

#### Intuition:

The problem asks us to find the maximum path sum in a binary tree. A path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root.

To solve this problem, the key insight is to recognize that for any given node, there are two main possibilities for the maximum path that includes that node:
1. The path goes through the node, extending into both its left and right subtrees.
2. The path only takes into account one of its subtrees (left or right).

Thus, for each node, we want to calculate:
- The maximum path sum with the node as the “pivot node” (the path passes through the node), called `path_through_node`.
- The maximum path sum from the node going one direction (either left or right), which is needed to propagate upwards.

By using a depth-first search (DFS) approach, we can recursively calculate these values for each node, updating a global maximum as needed.

#### Code:

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
        # Global variable to store the maximum path sum
        self.max_sum = float('-inf')
        
        def dfs(node):
            if not node:
                return 0

            # Calculate maximum path sum from left and right subtrees
            left_max = max(dfs(node.left), 0)  # ignoring paths that contribute negatively
            right_max = max(dfs(node.right), 0)  # ignoring paths that contribute negatively
            
            # Current path sum that includes the current node 
            path_through_node = node.val + left_max + right_max
            
            # Update the global maximum sum if needed
            self.max_sum = max(self.max_sum, path_through_node)
            
            # Return the maximum path sum of either left or right subtree plus the node's value
            # This is returned for parent nodes to use and thus, does not include both subtrees
            return node.val + max(left_max, right_max)
        
        dfs(root)
        return self.max_sum
```

#### Complexity Analysis:

- **Time Complexity:** `O(N)`, where `N` is the number of nodes in the tree. In the worst case, we visit each node exactly once.
- **Space Complexity:** `O(H)`, where `H` is the height of the tree. The space complexity is the space used by the recursion stack. In the worst case (a skewed tree), it could be `O(N)`, while in the best case (a balanced tree), it could be `O(log N)`.

