# [Leetcode 1026: Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: DFS with Min and Max Values](#approach-2-dfs-with-min-and-max-values)

### Approach 1: Brute Force

**Intuition:**

A straightforward method is to iterate through each node in the tree and check all its ancestors to compute the required difference. This involves traversing the tree for every node to gather its ancestors and compute the difference.

However, since this involves not only finding the node but also re-traversing back up to get all ancestors each time, it can be inefficient especially with larger trees.

**Steps:**
1. For each node in the tree, navigate back up the tree to get all of its ancestors.
2. Compute the absolute difference between the node and each of its ancestors.
3. Keep track of the maximum difference found for each node.
4. Return the overall maximum difference.

**Complexity Analysis:**
- **Time Complexity:** \(O(N^2)\), where \(N\) is the number of nodes in the tree. For each node, potentially traversing back to the root to gather ancestors.
- **Space Complexity:** \(O(H)\), where \(H\) is the height of the tree, due to recursion stack depth.

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def maxAncestorDiff(self, root: TreeNode) -> int:
        def getAncestors(node, ancestors):
            if not node:
                return 0
            max_diff = 0
            # Compare node with all ancestors
            for ancestor in ancestors:
                max_diff = max(max_diff, abs(node.val - ancestor))
            # Update ancestors list & recurse to children
            ancestors.append(node.val)
            max_diff = max(max_diff, getAncestors(node.left, ancestors))
            max_diff = max(max_diff, getAncestors(node.right, ancestors))
            ancestors.pop()
            return max_diff
        
        return getAncestors(root, [])
```

### Approach 2: DFS with Min and Max Values

**Intuition:**

Instead of keeping a list of all ancestors, we only need to track the minimum and maximum values as we traverse from the root to the leaf. This is because the largest possible difference for a node is always derived from these extremes of its ancestors.

For each node, we calculate potential differences using its value against both the current minimum and maximum values encountered so far.

**Steps:**
1. Start from the root with initial min and max values as the root's value.
2. For each node, update the min and max values if the current node's value is less or more, respectively.
3. Compute differences between the node's value and the min/max values.
4. Recursively compute for left and right subtrees.
5. Return the maximum difference found.

**Complexity Analysis:**
- **Time Complexity:** \(O(N)\), where \(N\) is the number of nodes, as each node is visited once.
- **Space Complexity:** \(O(H)\), where \(H\) is the height of the tree, due to recursion stack depth.

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def maxAncestorDiff(self, root: TreeNode) -> int:
        def dfs(node, curr_min, curr_max):
            if not node:
                return curr_max - curr_min
            
            # Update min and max for this path
            curr_min = min(curr_min, node.val)
            curr_max = max(curr_max, node.val)
            
            # Recursively compute for children
            left_diff = dfs(node.left, curr_min, curr_max)
            right_diff = dfs(node.right, curr_min, curr_max)
            
            # Return the max diff found
            return max(left_diff, right_diff)
        
        return dfs(root, root.val, root.val)
```

For the given problem, Approach 2 provides an optimal solution in terms of both time and space, efficiently using DFS traversal to maintain a running minimum and maximum value for each path in the tree.

