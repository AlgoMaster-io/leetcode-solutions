# [Leetcode 427: Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Recursive Approach with Optimization](#recursive-approach-with-optimization)

### Recursive Approach

Intuition:
The quad tree is a specialized tree used to partition a two-dimensional space by recursively subdividing it into four quadrants or regions. To construct a quad tree from a grid, we recursively divide the grid into four quadrants until each quadrant is a uniform section. If a section is not uniform (contains both 0s and 1s), we continue dividing it; otherwise, we create a leaf node.

#### Code:
```java
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;

    public Node(boolean val, boolean isLeaf) {
        this.val = val;
        this.isLeaf = isLeaf;
    }

    public Node(boolean val, boolean isLeaf, Node topLeft, Node topRight, Node bottomLeft, Node bottomRight) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = topLeft;
        this.topRight = topRight;
        this.bottomLeft = bottomLeft;
        this.bottomRight = bottomRight;
    }
}

class Solution {
    public Node construct(int[][] grid) {
        return construct(grid, 0, 0, grid.length);
    }

    private Node construct(int[][] grid, int row, int col, int size) {
        // Check if the entire grid section is uniform
        if (isUniform(grid, row, col, size)) {
            return new Node(grid[row][col] == 1, true);
        }

        int mid = size / 2;
        // Recursively divide into four quadrants
        Node topLeft = construct(grid, row, col, mid);
        Node topRight = construct(grid, row, col + mid, mid);
        Node bottomLeft = construct(grid, row + mid, col, mid);
        Node bottomRight = construct(grid, row + mid, col + mid, mid);

        // Create a parent node for the 4 quadrants
        return new Node(false, false, topLeft, topRight, bottomLeft, bottomRight);
    }

    private boolean isUniform(int[][] grid, int row, int col, int size) {
        int val = grid[row][col];
        for (int i = row; i < row + size; i++) {
            for (int j = col; j < col + size; j++) {
                if (grid[i][j] != val) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

#### Complexity:
- **Time Complexity**: O(N^2), where N is the length of the grid's side. In the worst case, we might check each cell to determine if a section is uniform.
- **Space Complexity**: O(log(N)), as the stack space used by the recursive calls at worst will be log of N due to the depth of the recursive tree.

### Recursive Approach with Optimization

Intuition:
To optimize, instead of checking for uniform sections separately, integrate this directly into the recursive logic to avoid redundant checks. If during recursive splitting, all four quadrants return leaf nodes with the same value, we can merge them into a single leaf node.

#### Code:
```java
class Solution {
    public Node construct(int[][] grid) {
        return construct(grid, 0, 0, grid.length);
    }

    private Node construct(int[][] grid, int row, int col, int size) {
        if (size == 1) {
            return new Node(grid[row][col] == 1, true);
        }

        int mid = size / 2;
        Node topLeft = construct(grid, row, col, mid);
        Node topRight = construct(grid, row, col + mid, mid);
        Node bottomLeft = construct(grid, row + mid, col, mid);
        Node bottomRight = construct(grid, row + mid, col + mid, mid);

        // Optimize by directly merging the nodes if they are all leaves and have the same value
        if (topLeft.isLeaf && topRight.isLeaf && bottomLeft.isLeaf && bottomRight.isLeaf
                && topLeft.val == topRight.val && topRight.val == bottomLeft.val && bottomLeft.val == bottomRight.val) {
            return new Node(topLeft.val, true);
        }

        return new Node(false, false, topLeft, topRight, bottomLeft, bottomRight);
    }
}
```

#### Complexity:
- **Time Complexity**: O(N^2), similar to the basic approach, but avoids redundant checks.
- **Space Complexity**: O(log(N)), due to recursive stack similar to the basic approach.

By integrating the uniform check within the recursive construction, we can reduce the overall checks thereby optimizing the performance without increasing the complexity in big O terms.

