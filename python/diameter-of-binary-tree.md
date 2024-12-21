# [Diameter of Binary Tree - Leetcode 543](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized DFS](#approach-2-optimized-dfs)

---

## Approach 1: Brute Force

### Intuition
The diameter of the binary tree is the length of the longest path between any two nodes in the tree. This path may or may not pass through the root. The brute force approach computes the diameter of the binary tree by evaluating the maximum path which can be computed for every node. For each node, calculate the height of the left and right subtrees and sum them up to consider that path as the potential diameter. This path sum is computed at every node to identify the maximum path among any two nodes in the tree.

### Steps:
1. Define a `height()` function that calculates the height of a given binary tree node recursively.
2. Traverse the tree for each node. For each node, compute the path that links the left and right subtrees.
3. Use the path length calculated at each node to assess if it's the longest diameter possible.
4. Return the maximum computed diameter.

### Code
```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def diameterOfBinaryTree(root: TreeNode) -> int:
    def height(node):
        # A null node has height 0
        if not node:
            return 0
        # Compute the height of each subtree
        left_height = height(node.left)
        right_height = height(node.right)
        # The height of the node is the greater of the two subtrees' heights plus one for the current node
        return 1 + max(left_height, right_height)
    
    def maxDiameter(node):
        # Base condition: if node is null, diameter is 0
        if not node:
            return 0
        # Sum the heights of the left and right children nodes
        left_height = height(node.left)
        right_height = height(node.right)
        # Compute diameter through this node
        current_diameter = left_height + right_height
        # Calculate diameter in the subtrees
        left_diameter = maxDiameter(node.left)
        right_diameter = maxDiameter(node.right)
        # Maximum of both the current diameter through this node and the maximum child diameter
        return max(current_diameter, left_diameter, right_diameter)
    
    return maxDiameter(root)
```

### Complexity
- **Time Complexity**: O(N^2), where N is the number of nodes in the tree. We recalculated height for each node.
- **Space Complexity**: O(H), where H is the height of the tree due to the recursion stack.

---

## Approach 2: Optimized DFS

### Intuition
Instead of recalculating the height of each node's subtree multiple times, we can opt for an optimized depth-first search which calculates both the diameter and height in the same traversal. We leverage a single traversal to compute the diameter passing through every node, maintaining a global variable to track the maximum diameter observed.

### Steps:
1. Define a recursive DFS function that computes both the diameter through the node and returns the height of the node.
2. For each node, compute the height of the subtrees. The diameter passing through that node is given by adding these heights.
3. Update the global maximum diameter whenever a larger diameter is found.
4. Return the computed maximum diameter after a complete traversal.

### Code
```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def diameterOfBinaryTree(root: TreeNode) -> int:
    def dfs(node):
        nonlocal max_diameter
        if not node:
            return 0  # Base case: return 0 for null nodes
        
        # Postorder traversal, calculate height of left subtree
        left_height = dfs(node.left)
        # Calculate height of right subtree
        right_height = dfs(node.right)

        # Calculate diameter through this node as sum of heights of left and right
        max_diameter = max(max_diameter, left_height + right_height)

        # Return height of current node
        return 1 + max(left_height, right_height)
    
    max_diameter = 0
    dfs(root)
    return max_diameter
```

### Complexity
- **Time Complexity**: O(N), where N is the number of nodes in the tree. Each node is visited once.
- **Space Complexity**: O(H), where H is the height of the tree due to the recursion stack.

This approach is optimal since it reduces redundant calculations of subtree heights, achieving a time complexity proportional to the number of nodes.

