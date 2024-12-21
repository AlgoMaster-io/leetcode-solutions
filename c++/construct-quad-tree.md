# [Leetcode 427: Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Approaches

- [Basic Approach: Recursive Build with Base Cases](#basic-approach)
- [Optimized Approach: Recursive and Pruning](#optimized-approach)

---

### Basic Approach: Recursive Build with Base Cases

The problem asks us to construct a Quad Tree from a given grid. A Quad Tree is a tree data structure in which each internal node has exactly four children. The tree structure allows us to represent a two-dimensional grid with varying element values efficiently.

#### Intuition

1. **Base Case**: If the grid size is 1x1, create a leaf node with the value of the single grid element.
2. **Recursive Case**: 
   - Divide the current grid into four equal subgrids.
   - Recursively build the Quad Tree for each of the subgrids.
   - Combine the results: If all four subtrees are leaves and have the same value, then they can be merged into a single leaf node with that value.

```cpp
// Definition for a QuadTree node.
class Node {
public:
    bool val;
    bool isLeaf;
    Node* topLeft;
    Node* topRight;
    Node* bottomLeft;
    Node* bottomRight;

    Node() {
        val = false;
        isLeaf = false;
        topLeft = nullptr;
        topRight = nullptr;
        bottomLeft = nullptr;
        bottomRight = nullptr;
    }

    Node(bool _val, bool _isLeaf) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = nullptr;
        topRight = nullptr;
        bottomLeft = nullptr;
        bottomRight = nullptr;
    }

    Node(bool _val, bool _isLeaf, Node* _topLeft, Node* _topRight, Node* _bottomLeft, Node* _bottomRight) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = _topLeft;
        topRight = _topRight;
        bottomLeft = _bottomLeft;
        bottomRight = _bottomRight;
    }
};

class Solution {
public:
    Node* construct(std::vector<std::vector<int>>& grid) {
        return construct(grid, 0, 0, grid.size());
    }
    
    Node* construct(std::vector<std::vector<int>>& grid, int x, int y, int length) {
        if (length == 1) {
            // Base case: Single element, create a leaf node
            return new Node(grid[x][y] == 1, true);
        }
        
        int half = length / 2;
        // Recursively construct subtrees for each quadrant
        Node* topLeft = construct(grid, x, y, half);
        Node* topRight = construct(grid, x, y + half, half);
        Node* bottomLeft = construct(grid, x + half, y, half);
        Node* bottomRight = construct(grid, x + half, y + half, half);
        
        // If all children are leaves and have the same value, merge them
        if (topLeft->isLeaf && topRight->isLeaf && bottomLeft->isLeaf && bottomRight->isLeaf &&
            topLeft->val == topRight->val && topRight->val == bottomLeft->val && bottomLeft->val == bottomRight->val) {
            return new Node(topLeft->val, true);
        } else {
            // Otherwise create an internal node with the four quadrants
            return new Node(false, false, topLeft, topRight, bottomLeft, bottomRight);
        }
    }
};
```

**Time Complexity**: O(N^2), where N is the side length of the grid. Each cell is visited at most once.

**Space Complexity**: O(N^2) in the worst case, as the tree size is proportional to the number of cells.

---

### Optimized Approach: Recursive and Pruning

This approach mirrors the basic approach, focusing on efficiency by directly merging regions that are uniformly filled, reducing unnecessary tree creation.

#### Intuition

The algorithm uses the same basic idea as before but ensures that unnecessary nodes are not created when the entire subgrid has a uniform value. The complexity remains similar due to the recursive nature and tree depth control corresponding to grid divisions.

**Note**: As the optimized version heavily depends on input data uniformity, this approach's tangible benefit surfaces in practice often in larger or more uniform grids rather than changing the asymptotic complexity.

The code will remain the same as the basic approach as it already captures the optimization through conditional merging by checking the leaf status and value uniformity.

**Time Complexity**: O(N^2)

**Space Complexity**: O(N^2)

This abstract representation ensures no repeated construction and maximal use of recursion optimization.

