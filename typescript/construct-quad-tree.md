# Leetcode Problem: [427. Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Navigation
- [Approach 1: Brute Force Recursive Approach](#approach-1-brute-force-recursive-approach)
- [Approach 2: Optimized Recursive Approach with Early Stopping](#approach-2-optimized-recursive-approach-with-early-stopping)

## Approach 1: Brute Force Recursive Approach

### Intuition:
The problem is about constructing a quad tree from a binary grid. A quad tree is a tree data structure in which each internal node has exactly four children. We can construct a quad tree recursively by dividing the grid into four quadrants. If all the values in the grid are the same (all 1s or all 0s), we create a leaf node with that value. Otherwise, we divide the grid and recurse on each quadrant.

### Steps:
1. Check if the current grid section (sub-grid) has a consistent value (all 0s or all 1s).
   - If consistent, create a leaf node.
   - If not consistent, divide the current grid into four equal parts and recursively construct trees for each part.
2. Each divide step reduces the size of the grid by 4, leading to completing the construction when the grid section size turns to 1.

### Code:

```typescript
class Node {
    val: boolean;
    isLeaf: boolean;
    topLeft: Node | null;
    topRight: Node | null;
    bottomLeft: Node | null;
    bottomRight: Node | null;

    constructor(val?: boolean, isLeaf?: boolean, topLeft?: Node, topRight?: Node, bottomLeft?: Node, bottomRight?: Node) {
        this.val = val === undefined ? false : val;
        this.isLeaf = isLeaf === undefined ? false : isLeaf;
        this.topLeft = topLeft === undefined ? null : topLeft;
        this.topRight = topRight === undefined ? null : topRight;
        this.bottomLeft = bottomLeft === undefined ? null : bottomLeft;
        this.bottomRight = bottomRight === undefined ? null : bottomRight;
    }
}

function construct(grid: number[][]): Node | null {
    // Helper function to determine if the sub-grid has the same values
    function isUniform(x: number, y: number, length: number): boolean {
        const value = grid[x][y];
        for (let i = x; i < x + length; i++) {
            for (let j = y; j < y + length; j++) {
                if (grid[i][j] !== value) {
                    return false;
                }
            }
        }
        return true;
    }

    // Recursive function to construct the quad tree
    function helper(x: number, y: number, length: number): Node {
        if (isUniform(x, y, length)) {
            // All values are the same in this sub-grid
            return new Node(grid[x][y] === 1, true);
        }
        
        // Create the internal node, divide the grid into quadrants
        const half = length / 2;
        return new Node(
            false,
            false,
            helper(x, y, half),
            helper(x, y + half, half),
            helper(x + half, y, half),
            helper(x + half, y + half, half)
        );
    }
    
    return helper(0, 0, grid.length);
}
```

### Complexity:
- **Time Complexity**: O(n^2 * log(n)), where n is the dimension of the grid. The log comes from the recursion depth and the n^2 from iterating over the list each time.
- **Space Complexity**: O(log(n)), which is the depth of the recursion.

## Approach 2: Optimized Recursive Approach with Early Stopping

### Intuition:
This approach is similar to the brute force recursive approach but adds the early stopping condition. When constructing the quad tree, it reduces unnecessary computations by immediately returning false if a discrepancy in values is found, avoiding checking all parts of the grid.

### Steps:
1. Use recursion with a similar breakdown of the grid.
2. Include an early stopping mechanism in the grid consistency check.
3. The constructed nodes will indicate if a grid section is further divisible or a leaf node.

### Code:

```typescript
function construct(grid: number[][]): Node | null {
    // Optimized helper function with early stopping
    function isUniform(x: number, y: number, length: number): boolean {
        const value = grid[x][y];
        for (let i = x; i < x + length; i++) {
            for (let j = y; j < y + length; j++) {
                if (grid[i][j] !== value) {
                    return false;
                }
            }
        }
        return true;
    }

    function helper(x: number, y: number, length: number): Node {
        // Early check for uniform sub-grid
        if (isUniform(x, y, length)) {
            return new Node(grid[x][y] === 1, true);
        }
        
        const half = length / 2;
        return new Node(
            false,
            false,
            helper(x, y, half),
            helper(x, y + half, half),
            helper(x + half, y, half),
            helper(x + half, y + half, half)
        );
    }
    
    return helper(0, 0, grid.length);
}
```

### Complexity:
- **Time Complexity**: O(n^2), as the early stopping refines the search, avoiding redundant checks.
- **Space Complexity**: O(log(n)), due to recursive call stack depth.

