# [Leetcode 54: Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approaches

- [Approach 1: Layer-by-layer Spiral Traversal](#approach-1-layer-by-layer-spiral-traversal)
- [Approach 2: Directional Spiral Traversal](#approach-2-directional-spiral-traversal)

### Approach 1: Layer-by-layer Spiral Traversal

#### Intuition

The Spiral Matrix problem can be effectively solved by visualizing the matrix as layers or rings. We can tackle the problem by processing one "layer" at a time and gradually moving inward towards the center of the matrix. This involves:

1. Traversing the top row from left to right.
2. Traversing the rightmost column from top to bottom.
3. Traversing the bottom row from right to left.
4. Traversing the leftmost column from bottom to top.

We shrink the boundaries and continue this process until the entire matrix is traversed.

#### Steps

1. Define the boundaries of the current layer with four variables: `top`, `bottom`, `left`, and `right`.
2. Initiate a loop that continues until `top` exceeds `bottom` or `left` exceeds `right`.
3. Within the loop:
   - Traverse from left to right across the top row.
   - Move the `top` boundary downward (as this row has been fully traversed).
   - Traverse from top to bottom down the right column.
   - Move the `right` boundary leftward.
   - Traverse from right to left along the bottom row if still within bounds.
   - Move the `bottom` boundary upward.
   - Traverse from bottom to top up the leftmost column if still within bounds.
   - Move the `left` boundary rightward.

#### Time and Space Complexity

- **Time Complexity**: O(m * n), where m is the number of rows and n is the number of columns. Each element is visited only once.
- **Space Complexity**: O(1) beyond input and output, as we only use a fixed amount of extra space.

```go
func spiralOrder(matrix [][]int) []int {
    if len(matrix) == 0 {
        return []int{}
    }

    var result []int
    top, bottom := 0, len(matrix)-1
    left, right := 0, len(matrix[0])-1

    for top <= bottom && left <= right {
        // Traverse from left to right across the top row.
        for i := left; i <= right; i++ {
            result = append(result, matrix[top][i])
        }
        top++

        // Traverse from top to bottom down the right column.
        for i := top; i <= bottom; i++ {
            result = append(result, matrix[i][right])
        }
        right--

        if top <= bottom {
            // Traverse from right to left along the bottom row.
            for i := right; i >= left; i-- {
                result = append(result, matrix[bottom][i])
            }
            bottom--
        }

        if left <= right {
            // Traverse from bottom to top up the left column.
            for i := bottom; i >= top; i-- {
                result = append(result, matrix[i][left])
            }
            left++
        }
    }

    return result
}
```

### Approach 2: Directional Spiral Traversal

#### Intuition

Instead of carefully managing the boundaries (top, bottom, left, right), we can follow a more dynamic and directional approach. This involves keeping track of direction and adaptively changing our course when hitting a boundary or a previously visited cell.

#### Steps

1. Use a directional vector `dirs` to manage direction: `right, down, left, up`.
2. Maintain indices and count through the traversal.
3. Start with direction `right` and change according to the current position and boundaries.
4. Continue till all elements are visited, using a 2D visited status array to ensure elements are processed only once.

#### Time and Space Complexity

- **Time Complexity**: O(m * n), as every element is visited once.
- **Space Complexity**: O(m * n) for the auxiliary visited matrix.

```go
func spiralOrder(matrix [][]int) []int {
    if len(matrix) == 0 {
        return []int{}
    }
    
    rows, cols := len(matrix), len(matrix[0])
    var result []int
    visited := make([][]bool, rows)
    for i := range visited {
        visited[i] = make([]bool, cols)
    }

    directions := [][2]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}} // right, down, left, up
    dirIndex := 0 
    row, col := 0, 0
    
    for i := 0; i < rows*cols; i++ {
        result = append(result, matrix[row][col])
        visited[row][col] = true
        newRow, newCol := row+directions[dirIndex][0], col+directions[dirIndex][1]
        
        if newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && !visited[newRow][newCol] {
            row, col = newRow, newCol
        } else {
            dirIndex = (dirIndex + 1) % 4 // change direction
            row += directions[dirIndex][0]
            col += directions[dirIndex][1]
        }
    }
    
    return result
}
```

By using both these methods, we provide a variety of approaches to grasp the problem of the spiral matrix, each with its own advantages in clarity and space efficiency.

