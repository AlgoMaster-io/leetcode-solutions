# [427. Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Approach 1: Recursive Division of Grid

### Solution
typescript
```typescript
// Time Complexity: O(n^2 * log n), where n is the side length of the grid
// Space Complexity: O(log n) (due to recursion stack)

class Node {
    val: boolean;
    isLeaf: boolean;
    topLeft: Node | null;
    topRight: Node | null;
    bottomLeft: Node | null;
    bottomRight: Node | null;

    constructor(val: boolean, isLeaf: boolean, topLeft: Node | null, topRight: Node | null, bottomLeft: Node | null, bottomRight: Node | null) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = topLeft;
        this.topRight = topRight;
        this.bottomLeft = bottomLeft;
        this.bottomRight = bottomRight;
    }
}

function construct(grid: number[][]): Node {
    return buildTree(grid, 0, 0, grid.length);
}

function buildTree(grid: number[][], x: number, y: number, size: number): Node {
    if (size === 1) {
        // Base case: single cell
        return new Node(grid[x][y] === 1, true, null, null, null, null);
    }

    const halfSize = size / 2;

    // Recursively divide the grid into four quadrants
    const topLeft = buildTree(grid, x, y, halfSize);
    const topRight = buildTree(grid, x, y + halfSize, halfSize);
    const bottomLeft = buildTree(grid, x + halfSize, y, halfSize);
    const bottomRight = buildTree(grid, x + halfSize, y + halfSize, halfSize);

    // Check if all four quadrants are leaves with the same value
    if (topLeft.isLeaf && topRight.isLeaf && bottomLeft.isLeaf && bottomRight.isLeaf
            && topLeft.val === topRight.val && topRight.val === bottomLeft.val
            && bottomLeft.val === bottomRight.val) {
        return new Node(topLeft.val, true, null, null, null, null); // Merge into one leaf
    }

    // Otherwise, create an internal node
    return new Node(true, false, topLeft, topRight, bottomLeft, bottomRight);
}
```

