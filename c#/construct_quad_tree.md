# [427. Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Approach 1: Recursive Division of Grid

### Solution
csharp
```csharp
// Time Complexity: O(n^2 * log n), where n is the side length of the grid
// Space Complexity: O(log n) (due to recursion stack)
public class Solution {
    public Node Construct(int[][] grid) {
        return BuildTree(grid, 0, 0, grid.Length);
    }

    private Node BuildTree(int[][] grid, int x, int y, int size) {
        if (size == 1) {
            // Base case: single cell
            return new Node(grid[x][y] == 1, true, null, null, null, null);
        }

        int halfSize = size / 2;

        // Recursively divide the grid into four quadrants
        Node topLeft = BuildTree(grid, x, y, halfSize);
        Node topRight = BuildTree(grid, x, y + halfSize, halfSize);
        Node bottomLeft = BuildTree(grid, x + halfSize, y, halfSize);
        Node bottomRight = BuildTree(grid, x + halfSize, y + halfSize, halfSize);
        
        // Check if all four quadrants are leaves with the same value
        if (topLeft.isLeaf && topRight.isLeaf && bottomLeft.isLeaf && bottomRight.isLeaf
                && topLeft.val == topRight.val && topRight.val == bottomLeft.val
                && bottomLeft.val == bottomRight.val) {
            return new Node(topLeft.val, true, null, null, null, null); // Merge into one leaf
        }

        // Otherwise, create an internal node
        return new Node(true, false, topLeft, topRight, bottomLeft, bottomRight);
    }
}

public class Node {
    public bool val;
    public bool isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;

    public Node() {
        this.val = false;
        this.isLeaf = false;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }

    public Node(bool val, bool isLeaf) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = null;
        this.topRight = null;
        this.bottomLeft = null;
        this.bottomRight = null;
    }

    public Node(bool val, bool isLeaf, Node topLeft, Node topRight, Node bottomLeft, Node bottomRight) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = topLeft;
        this.topRight = topRight;
        this.bottomLeft = bottomLeft;
        this.bottomRight = bottomRight;
    }
}
```

