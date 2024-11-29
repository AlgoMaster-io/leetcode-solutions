# [427. Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Approach 1: Recursive Division of Grid

### Solution
```python
# Time Complexity: O(n^2 * log n), where n is the side length of the grid
# Space Complexity: O(log n) (due to recursion stack)
class Node:
    def __init__(self, val, isLeaf, topLeft, topRight, bottomLeft, bottomRight):
        self.val = val
        self.isLeaf = isLeaf
        self.topLeft = topLeft
        self.topRight = topRight
        self.bottomLeft = bottomLeft
        self.bottomRight = bottomRight

class Solution:
    def construct(self, grid):
        return self.build_tree(grid, 0, 0, len(grid))

    def build_tree(self, grid, x, y, size):
        if size == 1:
            # Base case: single cell
            return Node(grid[x][y] == 1, True, None, None, None, None)

        half_size = size // 2

        # Recursively divide the grid into four quadrants
        top_left = self.build_tree(grid, x, y, half_size)
        top_right = self.build_tree(grid, x, y + half_size, half_size)
        bottom_left = self.build_tree(grid, x + half_size, y, half_size)
        bottom_right = self.build_tree(grid, x + half_size, y + half_size, half_size)

        # Check if all four quadrants are leaves with the same value
        if (top_left.isLeaf and top_right.isLeaf and bottom_left.isLeaf and bottom_right.isLeaf
                and top_left.val == top_right.val and top_right.val == bottom_left.val
                and bottom_left.val == bottom_right.val):
            return Node(top_left.val, True, None, None, None, None)  # Merge into one leaf

        # Otherwise, create an internal node
        return Node(True, False, top_left, top_right, bottom_left, bottom_right)
```

