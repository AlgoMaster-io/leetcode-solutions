## [Construct Quad Tree - LeetCode 427](https://leetcode.com/problems/construct-quad-tree/)

### Solutions 
- [Naive Recursive Approach](#naive-recursive-approach)
- [Optimized Recursive Approach](#optimized-recursive-approach)

### Naive Recursive Approach

#### Intuition
The problem requires us to construct a quad tree from a 2D grid of 0s and 1s. A Quad-Tree is a tree data structure in which each internal node has exactly four children. The given grid can be broken down into four quadrants or regions: top-left, top-right, bottom-left, and bottom-right. We can recursively construct the quad tree by splitting the grid until we find a region that consists entirely of 0s or 1s, which would then become a leaf node.

#### Important Points:
- If the current grid cell is all ones or all zeros, it becomes a leaf.
- Otherwise, we partition the current grid into four equal-sized quadrants and recursively build the tree.

```javascript
// Definition for a QuadTree node.
class Node {
    constructor(val, isLeaf, topLeft, topRight, bottomLeft, bottomRight) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = topLeft;
        this.topRight = topRight;
        this.bottomLeft = bottomLeft;
        this.bottomRight = bottomRight;
    }
}

var construct = function(grid) {

    // Helper function to check if all elements in the grid are the same.
    const isUniform = (x, y, length) => {
        const firstVal = grid[x][y];
        for (let i = x; i < x + length; i++) {
            for (let j = y; j < y + length; j++) {
                if (grid[i][j] !== firstVal) {
                    return false;
                }
            }
        }
        return true;
    };

    // Recursive function to construct the Quad Tree.
    const buildQuadTree = (x, y, length) => {
        if (isUniform(x, y, length)) {
            // All cells in the current grid area are the same
            return new Node(grid[x][y] === 1, true, null, null, null, null);
        }

        const halfLength = length / 2;
        return new Node(
            '*',  // arbitrary value, won’t be used
            false,
            buildQuadTree(x, y, halfLength),
            buildQuadTree(x, y + halfLength, halfLength),
            buildQuadTree(x + halfLength, y, halfLength),
            buildQuadTree(x + halfLength, y + halfLength, halfLength)
        );
    };

    return buildQuadTree(0, 0, grid.length);
};
```

#### Time Complexity
- The time complexity is O(N^2 * log(N)) where N is the size of the grid. In the worst case, we visit every cell multiple times as we recursively divide the grid.
  
#### Space Complexity
- The space complexity is O(log(N)) due to the recursive stack depth as we partition the grid.

### Optimized Recursive Approach

#### Intuition
In the optimized solution, the approach remains essentially the same but with additional optimizations. We can avoid unnecessary checks by short-circuiting the uniform check or leveraging caching mechanisms. This example mainly focuses on clear and structural optimizations that can aid in slightly reducing the computational burden if any pre-computation is available.

#### Important Points:
- Making an early exit if part of the quadrant is known to be uniform aids in reducing computation time.
- Although practical real-world improvements can be marginal, logically structuring presence checks helps in different contexts efficiently.

```javascript
// Definition for a QuadTree node (As before)
class Node {
    constructor(val, isLeaf, topLeft, topRight, bottomLeft, bottomRight) {
        this.val = val;
        this.isLeaf = isLeaf;
        this.topLeft = topLeft;
        this.topRight = topRight;
        this.bottomLeft = bottomLeft;
        this.bottomRight = bottomRight;
    }
}

var construct = function(grid) {
    const cache = {};

    const isUniform = (x, y, length) => {
        const key = `${x}-${y}-${length}`;
        if (cache[key] !== undefined) {
            return cache[key];
        }

        const firstVal = grid[x][y];
        for (let i = x; i < x + length; i++) {
            for (let j = y; j < y + length; j++) {
                if (grid[i][j] !== firstVal) {
                    cache[key] = false;
                    return false;
                }
            }
        }
        cache[key] = true;
        return true;
    };

    const buildQuadTree = (x, y, length) => {
        if (isUniform(x, y, length)) {
            return new Node(grid[x][y] === 1, true, null, null, null, null);
        }

        const halfLength = length / 2;
        return new Node(
            '*',  // Not used, merely indicative
            false,
            buildQuadTree(x, y, halfLength),
            buildQuadTree(x, y + halfLength, halfLength),
            buildQuadTree(x + halfLength, y, halfLength),
            buildQuadTree(x + halfLength, y + halfLength, halfLength)
        );
    };

    return buildQuadTree(0, 0, grid.length);
};
```

#### Time Complexity
- The time complexity remains O(N^2 * log(N)) similar to the previous approach, as we still need to check many grid parts.

#### Space Complexity
- The space complexity is O(log(N)) primarily due to recursive calls and minor additional cache storage.

Both solutions essentially form the basis of constructing a Quad-Tree. The inclusion of caching (as shown in the optimized solution) can serve in larger practical datasets than basic theoretical exploration.

