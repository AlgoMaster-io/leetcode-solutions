# [427. Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Approach 1: Recursive Division of Grid

### Solution
Go
```go
// Time Complexity: O(n^2 * log n), where n is the side length of the grid
// Space Complexity: O(log n) (due to recursion stack)

// Definition for a QuadTree node.
type Node struct {
    Val         bool
    IsLeaf      bool
    TopLeft     *Node
    TopRight    *Node
    BottomLeft  *Node
    BottomRight *Node
}

func construct(grid [][]int) *Node {
    return buildTree(grid, 0, 0, len(grid))
}

func buildTree(grid [][]int, x, y, size int) *Node {
    if size == 1 {
        // Base case: single cell
        return &Node{Val: grid[x][y] == 1, IsLeaf: true}
    }

    halfSize := size / 2

    // Recursively divide the grid into four quadrants
    topLeft := buildTree(grid, x, y, halfSize)
    topRight := buildTree(grid, x, y+halfSize, halfSize)
    bottomLeft := buildTree(grid, x+halfSize, y, halfSize)
    bottomRight := buildTree(grid, x+halfSize, y+halfSize, halfSize)

    // Check if all four quadrants are leaves with the same value
    if topLeft.IsLeaf && topRight.IsLeaf && bottomLeft.IsLeaf && bottomRight.IsLeaf &&
        topLeft.Val == topRight.Val && topRight.Val == bottomLeft.Val &&
        bottomLeft.Val == bottomRight.Val {
        return &Node{Val: topLeft.Val, IsLeaf: true} // Merge into one leaf
    }

    // Otherwise, create an internal node
    return &Node{
        Val:         true,
        IsLeaf:      false,
        TopLeft:     topLeft,
        TopRight:    topRight,
        BottomLeft:  bottomLeft,
        BottomRight: bottomRight,
    }
}
```

