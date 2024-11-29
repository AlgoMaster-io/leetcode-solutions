# [427. Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Approach 1: Recursive Division of Grid

### Solution
```javascript
// Time Complexity: O(n^2 * log n), where n is the side length of the grid
// Space Complexity: O(log n) (due to recursion stack)

function Node(val, isLeaf, topLeft, topRight, bottomLeft, bottomRight) {
    this.val = val;
    this.isLeaf = isLeaf;
    this.topLeft = topLeft;
    this.topRight = topRight;
    this.bottomLeft = bottomLeft;
    this.bottomRight = bottomRight;
}

var construct = function(grid) {
    return buildTree(grid, 0, 0, grid.length);
};

function buildTree(grid, x, y, size) {
    if (size === 1) {
        // Base case: single cell
        return new Node(grid[x][y] === 1, true, null, null, null, null);
    }

    var halfSize = size / 2;
    
    // Recursively divide the grid into four quadrants
    var topLeft = buildTree(grid, x, y, halfSize);
    var topRight = buildTree(grid, x, y + halfSize, halfSize);
    var bottomLeft = buildTree(grid, x + halfSize, y, halfSize);
    var bottomRight = buildTree(grid, x + halfSize, y + halfSize, halfSize);

    // Check if all four quadrants are leaves with the same value
    if (topLeft.isLeaf && topRight.isLeaf && bottomLeft.isLeaf && bottomRight.isLeaf &&
        topLeft.val === topRight.val && topRight.val === bottomLeft.val &&
        bottomLeft.val === bottomRight.val) {
        return new Node(topLeft.val, true, null, null, null, null); // Merge into one leaf
    }

    // Otherwise, create an internal node
    return new Node(true, false, topLeft, topRight, bottomLeft, bottomRight);
}
```

