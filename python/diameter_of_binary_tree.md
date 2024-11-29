# 543. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approach 1: Depth-First Search (DFS) with Recursive Depth Calculation

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack

class Solution:
    def __init__(self):
        self.diameter = 0

    def diameterOfBinaryTree(self, root):
        self.depth(root)
        return self.diameter

    def depth(self, node):
        if node is None:
            return 0  # Base case: null node has depth 0

        leftDepth = self.depth(node.left)  # Calculate depth of left subtree
        rightDepth = self.depth(node.right)  # Calculate depth of right subtree

        # Update the diameter (maximum path length between two nodes)
        self.diameter = max(self.diameter, leftDepth + rightDepth)

        # Return the depth of the current subtree
        return max(leftDepth, rightDepth) + 1
```

## Approach 2: Bottom-Up DFS with Global Variable (Optimized)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for recursion stack

class Solution:
    def __init__(self):
        self.maxDiameter = 0

    def diameterOfBinaryTree(self, root):
        self.calculateDepth(root)
        return self.maxDiameter

    def calculateDepth(self, node):
        if node is None:
            return 0  # Null nodes contribute 0 to depth

        left = self.calculateDepth(node.left)  # Left subtree depth
        right = self.calculateDepth(node.right)  # Right subtree depth

        self.maxDiameter = max(self.maxDiameter, left + right)  # Update the max diameter

        return max(left, right) + 1  # Return the current depth
```

## Approach 3: Iterative DFS Using Stack

### Solution
python
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(h), where h is the height of the tree for the stack

class Solution:
    def diameterOfBinaryTree(self, root):
        if root is None:
            return 0  # Edge case: empty tree

        stack = []
        depthMap = {}
        maxDiameter = 0

        stack.append(root)
        while stack:
            current = stack[-1]
            if current is None:
                stack.pop()
                continue

            if (current.left and current.left not in depthMap) or (current.right and current.right not in depthMap):
                if current.left:
                    stack.append(current.left)
                if current.right:
                    stack.append(current.right)
            else:
                stack.pop()
                left = depthMap.get(current.left, 0)
                right = depthMap.get(current.right, 0)
                maxDiameter = max(maxDiameter, left + right)
                depthMap[current] = max(left, right) + 1

        return maxDiameter
```

