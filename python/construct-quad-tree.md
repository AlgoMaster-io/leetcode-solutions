# [Leetcode 427: Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Optimized Recursive Approach](#optimized-recursive-approach)

---

## Recursive Approach

### Intuition
A quad tree is a tree data structure used to partition a two-dimensional space by recursively subdividing it into four quadrants or regions. The problem involves constructing such a tree from a given `n x n` grid of integers, where `n` is a power of two.

The most straightforward way to construct the quad tree is through a recursive method:
- Divide the current square into four smaller squares (quadrants).
- For each quadrant, if all elements in that region are the same, it can be represented as a leaf node.
- Otherwise, recursively partition further and represent it with an internal node.

### Implementation

```python
"""
# Definition for a QuadTree node.
class Node:
    def __init__(self, val, isLeaf, topLeft, topRight, bottomLeft, bottomRight):
        self.val = val
        self.isLeaf = isLeaf
        self.topLeft = topLeft
        self.topRight = topRight
        self.bottomLeft = bottomLeft
        self.bottomRight = bottomRight
"""

def construct(grid):
    def construct_rec(x1, y1, x2, y2):
        # Helper function to check if all elements in the region are same
        def is_uniform():
            initial = grid[x1][y1]
            for i in range(x1, x2):
                for j in range(y1, y2):
                    if grid[i][j] != initial:
                        return False
            return True
        
        if is_uniform():  # Base case: if all elements are the same, form a leaf
            return Node(grid[x1][y1] == 1, True, None, None, None, None)

        # Recursive division into four quadrants
        mid_x, mid_y = (x1 + x2) // 2, (y1 + y2) // 2
        topLeft = construct_rec(x1, y1, mid_x, mid_y)
        topRight = construct_rec(x1, mid_y, mid_x, y2)
        bottomLeft = construct_rec(mid_x, y1, x2, mid_y)
        bottomRight = construct_rec(mid_x, mid_y, x2, y2)

        # Form an internal node since the current square isn't uniform
        return Node(False, False, topLeft, topRight, bottomLeft, bottomRight)
    
    n = len(grid)
    return construct_rec(0, 0, n, n)
```

### Complexity
- **Time Complexity:** \(O(n^2 \log n)\) due to recursive partitioning of each region and check for uniformity.
- **Space Complexity:** \(O(\log n)\) for recursive call stack.

---

## Optimized Recursive Approach

### Intuition
To optimize further, we use memoization to avoid repeatedly calculating whether a sub-grid is uniform. By storing results of previous checks, the approach efficiently checks larger grids that might have been previously evaluated.

### Implementation

```python
def construct(grid):
    memo = {}
    
    def is_uniform(x1, y1, x2, y2):
        if (x1, y1, x2, y2) in memo:
            return memo[(x1, y1, x2, y2)]
        
        initial = grid[x1][y1]
        for i in range(x1, x2):
            for j in range(y1, y2):
                if grid[i][j] != initial:
                    memo[(x1, y1, x2, y2)] = False
                    return False
        memo[(x1, y1, x2, y2)] = True
        return True

    def construct_rec(x1, y1, x2, y2):
        if is_uniform(x1, y1, x2, y2):  # Base case: if all elements are uniform, form a leaf
            return Node(grid[x1][y1] == 1, True, None, None, None, None)

        # Recursive division into quadrants
        mid_x, mid_y = (x1 + x2) // 2, (y1 + y2) // 2
        topLeft = construct_rec(x1, y1, mid_x, mid_y)
        topRight = construct_rec(x1, mid_y, mid_x, y2)
        bottomLeft = construct_rec(mid_x, y1, x2, mid_y)
        bottomRight = construct_rec(mid_x, mid_y, x2, y2)

        return Node(False, False, topLeft, topRight, bottomLeft, bottomRight)
    
    n = len(grid)
    return construct_rec(0, 0, n, n)
```

### Complexity
- **Time Complexity:** Reduced to approximately \(O(n^2)\) in practice, due to memoization avoiding redundant uniform checks.
- **Space Complexity:** \(O(n^2)\) as memoization stores result states for potential every sub-grid.

