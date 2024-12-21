# [Leetcode 427: Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Approaches:
- [Approach 1: Recursive Division (Top-Down)](#approach-1)
- [Approach 2: Optimized Recursive Division with Precheck](#approach-2)

---

## Approach 1: Recursive Division (Top-Down)

### Intuition:
The problem involves constructing a Quad-Tree from a square matrix. The idea is to recursively divide the matrix into four quadrants until each quadrant has a uniform value (either 0 or 1), or it cannot be divided further. This approach uses a direct recursive strategy to build the tree.

### Algorithm:
1. **Base Case**: If the matrix size `n` is 1, return a leaf node with `isLeaf` set to `true` and `val` set to the matrix's single value.
2. **Quadrants**:
   - Divide the matrix into four equal-sized quadrants.
   - Recursively construct the quad tree for each quadrant.
3. **Merge**:
   - If all four children are leaves and their values are the same, merge them into a single leaf node.
   - Otherwise, construct an internal node with these four children.
4. Return the constructed node.

### Complexity:
- **Time complexity**: O(n^2 log n) - Each level constructs 4 subproblems, and log n levels in the tree.
- **Space complexity**: O(log n) - Due to recursive calls.

### Code:

```go
type Node struct {
    Val     bool
    IsLeaf  bool
    TopLeft *Node
    TopRight *Node
    BottomLeft *Node
    BottomRight *Node
}

func construct(grid [][]int) *Node {
    return constructHelper(grid, 0, 0, len(grid))
}

func constructHelper(grid [][]int, x, y, size int) *Node {
    // Base case: if the grid size is 1, it should be a leaf
    if size == 1 {
        return &Node{
            Val:    grid[x][y] == 1,
            IsLeaf: true,
        }
    }
    
    halfSize := size / 2
    
    // Divide the grid into four quadrants
    topLeft := constructHelper(grid, x, y, halfSize)
    topRight := constructHelper(grid, x, y+halfSize, halfSize)
    bottomLeft := constructHelper(grid, x+halfSize, y, halfSize)
    bottomRight := constructHelper(grid, x+halfSize, y+halfSize, halfSize)
    
    // Check if all quadrants are leaves with the same value
    if topLeft.IsLeaf && topRight.IsLeaf && bottomLeft.IsLeaf && bottomRight.IsLeaf &&
        topLeft.Val == topRight.Val && topRight.Val == bottomLeft.Val && bottomLeft.Val == bottomRight.Val {
        return &Node{
            Val:    topLeft.Val,
            IsLeaf: true,
        }
    }
    
    // If not, return an internal node
    return &Node{
        IsLeaf:      false,
        TopLeft:     topLeft,
        TopRight:    topRight,
        BottomLeft:  bottomLeft,
        BottomRight: bottomRight,
    }
}
```

---

## Approach 2: Optimized Recursive Division with Precheck

### Intuition:
Before diving into recursion, we can first check if the entire subgrid is uniform. If it is, we can create a leaf node right away without further divisions. This optimization helps cut down unnecessary recursive calls when subgrids are uniform.

### Algorithm:
1. **Precheck for Uniformity**: Before diving into recursion, check if all elements in the current subgrid are the same. If so, directly return a leaf node.
2. **Otherwise**, follow the same recursive division and merging process as Approach 1.

### Complexity:
- **Time complexity**: O(n^2) - We traverse each cell once with premature exits on uniform grids.
- **Space complexity**: O(log n) - Due to recursive calls.

### Code:

```go
func constructOptimized(grid [][]int) *Node {
    return constructOptimizedHelper(grid, 0, 0, len(grid))
}

func constructOptimizedHelper(grid [][]int, x, y, size int) *Node {
    uniform := true
    firstVal := grid[x][y]
    
    // Pre-check for entire sub-grid uniformity: O(size * size)
    for i := 0; i < size; i++ {
        for j := 0; j < size; j++ {
            if grid[x+i][y+j] != firstVal {
                uniform = false
                break
            }
        }
        if !uniform {
            break
        }
    }
    
    // If the grid is uniform, return a leaf node
    if uniform {
        return &Node{
            Val:    firstVal == 1,
            IsLeaf: true,
        }
    }
    
    halfSize := size / 2
    
    // Divide the grid into four quadrants
    topLeft := constructOptimizedHelper(grid, x, y, halfSize)
    topRight := constructOptimizedHelper(grid, x, y+halfSize, halfSize)
    bottomLeft := constructOptimizedHelper(grid, x+halfSize, y, halfSize)
    bottomRight := constructOptimizedHelper(grid, x+halfSize, y+halfSize, halfSize)
    
    // Return an internal node with these quadrants as children
    return &Node{
        IsLeaf:      false,
        TopLeft:     topLeft,
        TopRight:    topRight,
        BottomLeft:  bottomLeft,
        BottomRight: bottomRight,
    }
}
```

In both approaches, the problem is effectively divided until a subgrid is uniform, ensuring an efficient construction of the Quad Tree. The optimized approach further optimizes it by leveraging pre-checks for uniformity to minimize unnecessary recursive calls.

